---
tags: [lld, problem, concurrency, rate-limiting, design-patterns]
aliases: [Rate Limiter, API Rate Limiter]
---

# Rate Limiter

Controls how many requests a client can make to an API within a time window. Exceeding the quota rejects the request.

---

## Requirements

1. Config loaded at startup — one config per endpoint, specifying algorithm + algorithm-specific params
2. Requests arrive as `(clientId, endpoint)` strings
3. Each endpoint uses its own algorithm instance enforcing per-client limits
4. Returns structured result: `(allowed, remaining, retryAfterMs)`
5. Unknown endpoint → fall back to a default config, don't reject

**Out of scope:** distributed rate limiting, dynamic config updates, metrics, config validation

---

## Entities & Relationships

**Request** — not an entity. Just two string parameters to `allow()`.
**Client / Endpoint** — not entities. Strings used as map keys.

| Entity | Responsibility |
|---|---|
| `RateLimiter` | Orchestrator. Receives `(clientId, endpoint)`, looks up the right `Limiter`, delegates to it. Owns the endpoint→limiter map and default limiter. |
| `LimiterFactory` | Creates the correct `Limiter` implementation from raw config data. |
| `Limiter` (interface) | Contract for all algorithms: `allow(key) → RateLimitResult`. Each implementation manages its own per-key state. |
| `RateLimitResult` | Immutable value object: `allowed`, `remaining`, `retryAfterMs` (null when allowed). |

```
RateLimiter
 ├── Map<endpoint, Limiter>
 │        └── TokenBucketLimiter | SlidingWindowLogLimiter | ...
 └── defaultLimiter: Limiter
```

---

## Class Design

```
class RateLimiter:
  - limiters: Map<string, Limiter>
  - defaultLimiter: Limiter

  + RateLimiter(configs[], defaultConfig)
  + allow(clientId, endpoint) -> RateLimitResult


class LimiterFactory:
  + create(configData) -> Limiter


interface Limiter:
  + allow(key) -> RateLimitResult


class RateLimitResult:
  - allowed: boolean
  - remaining: int
  - retryAfterMs: long | null

  + RateLimitResult(allowed, remaining, retryAfterMs)
```

> [!tip] Why a factory?
> Config is heterogeneous — different algorithms need different params. The factory centralizes the switch-on-algorithm-type logic so nothing else has to care.

> [!tip] Why an interface (not abstract base class)?
> Different algorithms need fundamentally different per-key state. Token Bucket needs `(tokens, lastRefillTime)`, Sliding Window Log needs `Queue<timestamp>`. No shared fields to pull up → interface keeps it clean.

> [!warning] retryAfterMs semantics
> Null when allowed (no need to retry). Positive millisecond value when denied. Don't use 0 as sentinel — it's ambiguous.

---

## Implementation

### `RateLimiter` constructor and `allow`

```
RateLimiter(configs, defaultConfig)
    factory = new LimiterFactory()
    limiters = {}
    for config in configs
        limiters[config["endpoint"]] = factory.create(config)
    defaultLimiter = factory.create(defaultConfig)

allow(clientId, endpoint)
    limiter = limiters.get(endpoint) ?? defaultLimiter
    return limiter.allow(clientId)
```

Eager instantiation — all limiters created at startup. Simpler than lazy creation and avoids race conditions on first access.

---

### `LimiterFactory.create`

```
create(config)
    algo = config["algorithm"]
    params = config["algoConfig"]

    switch algo
        case "TokenBucket":
            return new TokenBucketLimiter(params["capacity"], params["refillRatePerSecond"])
        case "SlidingWindowLog":
            return new SlidingWindowLogLimiter(params["maxRequests"], params["windowMs"])
        default:
            throw IllegalArgumentException("Unknown algorithm: " + algo)
```

---

### Algorithm: Token Bucket

Clients have a bucket of tokens. Requests consume one token. Tokens refill at a constant rate. Allows bursts up to capacity.

**Per-key state:** `(tokens: double, lastRefillTime: long)`
Uses `double` for tokens because fractional tokens accumulate between requests. Refill is computed on-demand — no background thread needed.

```
class TokenBucketLimiter:
    capacity: int
    refillRatePerSecond: int
    buckets: Map<string, TokenBucket>   // created on first access per key

    allow(key)
        bucket = getOrCreate(key)        // starts full

        now = currentTimeMillis()
        elapsed = now - bucket.lastRefillTime
        bucket.tokens = min(capacity, bucket.tokens + (elapsed * refillRatePerSecond / 1000))
        bucket.lastRefillTime = now

        if bucket.tokens >= 1
            bucket.tokens -= 1
            return RateLimitResult(true, floor(bucket.tokens), null)
        else
            tokensNeeded = 1 - bucket.tokens
            retryMs = ceil(tokensNeeded * 1000 / refillRatePerSecond)
            return RateLimitResult(false, 0, retryMs)
```

> [!tip] retryAfterMs math
> `tokensNeeded = 1 - bucket.tokens`. Wait `tokensNeeded / refillRate` seconds = `tokensNeeded * 1000 / refillRatePerSecond` ms. Round up with `ceil` to avoid telling clients to retry too soon.

---

### Algorithm: Sliding Window Log

Tracks exact timestamps of every request in a rolling window. Precise but memory-heavy — O(maxRequests) per client.

**Per-key state:** `Queue<long>` of timestamps (front = oldest, back = newest)

```
class SlidingWindowLogLimiter:
    maxRequests: int
    windowMs: long
    logs: Map<string, Queue<long>>      // created on first access per key

    allow(key)
        log = getOrCreate(key)

        now = currentTimeMillis()
        cutoff = now - windowMs

        // Remove stale timestamps outside the window
        while log.isNotEmpty() && log.peek() < cutoff
            log.poll()

        if log.size() < maxRequests
            log.add(now)
            return RateLimitResult(true, maxRequests - log.size(), null)
        else
            retryMs = (log.peek() + windowMs) - now   // when oldest timestamp ages out
            return RateLimitResult(false, 0, retryMs)
```

> [!tip] retryAfterMs math
> The oldest timestamp (front of queue) will fall outside the window at `oldest + windowMs`. Time until then = `(oldest + windowMs) - now`.

---

## Algorithm Comparison

| Algorithm | Per-key state | Allows bursts? | Memory | Accuracy |
|---|---|---|---|---|
| Token Bucket | `(double, long)` | Yes, up to capacity | O(1) | Approximate |
| Sliding Window Log | `Queue<timestamp>` | No | O(maxRequests) | Exact |
| Fixed Window Counter | `(int, long)` | At window boundary | O(1) | Boundary artifacts |
| Sliding Window Counter | `(int, int, long)` | No | O(1) | Near-exact |

---

## Verification Trace

```
Config: "/search" → TokenBucket(capacity=10, refillRate=1/s)

t=0: allow("user123", "/search")
  → getOrCreate → bucket{tokens=10, lastRefill=0}
  → elapsed=0, tokensToAdd=0
  → tokens=10 >= 1 → consume → tokens=9
  → RateLimitResult(true, 9, null) ✓

t=500ms: allow("user123", "/search")
  → elapsed=500, tokensToAdd=0.5
  → tokens=min(10, 9+0.5)=9.5, lastRefill=500
  → tokens >= 1 → consume → tokens=8.5
  → RateLimitResult(true, 8, null) ✓

t=1100ms: allow("user123", "/search")  [bucket near-empty after rapid requests: tokens=0.1]
  → elapsed=100, tokensToAdd=0.1 → tokens=0.2
  → tokens < 1 → denied
  → tokensNeeded=0.8, retryMs=ceil(800/1)=800
  → RateLimitResult(false, 0, 800) ✓
```

---

## Extensibility

**Add a new algorithm** — implement `Limiter`, add one `case` to the factory. Nothing else changes.

**Thread safety** — use per-key locking, not a global lock. Different clients check concurrently; only same-client requests block each other.

```python
# Per-key locking pattern
with self._buckets_lock:             # protect bucket creation
    bucket = getOrCreate(key)
with bucket.lock:                    # protect per-client state
    # refill, check, consume
```

> [!tip] Why per-key, not global?
> A global lock serializes all clients. Per-key locking means Client A and Client B are checked concurrently — they only block each other when they're the same client, which is correct.

**Dynamic config updates** — swap out the limiter instance atomically while preserving per-client state (clients in flight finish with old limiter; new requests use new one).

**Memory growth** — buckets/logs maps grow unbounded as new clients appear. Fix: evict entries inactive for > N minutes via a background sweep, or use an LRU cache with a fixed capacity. Evicted clients start fresh on next request (full bucket / empty log).

---

## Related
- [[> LLD Delivery Framework]]
- [[Concurrency - Correctness]]
- [[Concurrency - Scarcity]]
- [[> LLD Design Patterns]]
- [[LLD - Class Design]]
