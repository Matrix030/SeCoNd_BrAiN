---
tags: [concurrency, lld, threads, scarcity, semaphore, connection-pool]
aliases: [Scarcity, Concurrency Scarcity]
---

# Concurrency - Scarcity

Managing limited resources when demand exceeds supply.

> [!tip] Different from correctness
> The danger isn't data corruption — it's that there simply aren't enough resources. 5 connections, 100 requests. Most threads must wait. The question is how that waiting is managed without the system falling over.

---

## The Two Solutions

### Semaphores

A counter that limits how many threads can do something concurrently. Create with N permits; threads `acquire` before working and `release` after. When permits hit zero, new threads block until one is released.

```python
import threading

class APIClient:
    def __init__(self):
        self._semaphore = threading.Semaphore(5)

    def make_request(self, endpoint: str):
        with self._semaphore:   # acquire on enter, release on exit (even on exception)
            return self._http_client.get(endpoint)
```

Use when: you need to cap concurrent operations but don't have actual objects to hand out. No "request object" exists — you just need at most 5 in flight.

> [!warning] Always release in a finally block
> The `with` statement handles this. If you write `acquire()`/`release()` manually, forgetting `finally` leaks permits permanently — the pool drains and blocks forever.

---

### Resource Pooling (Blocking Queue)

When the scarce resource is a stateful object (connection, GPU context, file handle), you need to hand out the actual objects and get them back. A blocking queue does this:

```python
import queue

class ConnectionPool:
    def __init__(self, pool_size: int, timeout_sec: float = 0.5):
        self._pool = queue.Queue(maxsize=pool_size)
        self._timeout = timeout_sec
        for _ in range(pool_size):
            self._pool.put(self._create_connection())

    def execute_query(self, query: str):
        try:
            conn = self._pool.get(timeout=self._timeout)
        except queue.Empty:
            raise RuntimeError("No connection available — pool exhausted")
        try:
            return conn.execute(query)
        finally:
            self._pool.put(conn)  # always return, even on exception
```

`get()` blocks when empty (all connections in use). `put()` in `finally` ensures no connection is ever lost. Use `get(timeout=N)` instead of `get()` to fail fast on request paths — blocking indefinitely causes user-visible hangs.

> [!warning] Always bound the queue
> `queue.Queue()` without `maxsize` is unbounded. Specify `maxsize=pool_size` so the pool can never hold more objects than it started with.

> [!tip] Why not just use a semaphore for a connection pool?
> A semaphore limits to N concurrent operations — but where do the actual Connection objects live? The blocking queue stores and dispenses the objects themselves. That's the difference.

**Initialization:** create all objects upfront in `__init__`. It's simpler, avoids lazy-init race conditions, and gives predictable performance once running.

**Timeouts:** tune to expected operation duration plus buffer. DB queries run in 100ms? Set 500ms. Stay under whatever timeout your load balancer has configured — better to return a proper error than let the LB give up first.

---

## The Four Patterns

### 1. Limit Concurrent Operations

Cap how many operations run at once. No actual resource objects — just a count.

**Use:** semaphore with N permits.

```python
class DownloadManager:
    def __init__(self):
        self._semaphore = threading.Semaphore(3)

    def download(self, url: str, destination):
        with self._semaphore:
            data = self._http_client.download(url)
            destination.write_bytes(data)
```

**Appears as:** download managers, rate-limited API wrappers, image/video processors, parking lot systems (spots as permits).

**Interview signal:** "limit to N concurrent X" → semaphore.

---

### 2. Limit Aggregate Consumption

Operations consume variable amounts of a shared budget (bandwidth, memory, I/O). The constraint is total units in use, not count of operations.

```python
class DiskWriter:
    MB = 1024 * 1024

    def __init__(self):
        self._condition = threading.Condition()
        self._available = 100  # 100 MB budget

    def write_file(self, data: bytes, path):
        permits = max(1, (len(data) + self.MB - 1) // self.MB)
        with self._condition:
            while self._available < permits:
                self._condition.wait()
            self._available -= permits
        try:
            path.write_bytes(data)
        finally:
            with self._condition:
                self._available += permits
                self._condition.notify_all()
```

Each operation acquires permits proportional to what it consumes. Small files get through quickly; large files wait for capacity.

> [!note] This limits concurrent bytes, not throughput rate
> True rate limiting (e.g. 100 MB/s) requires time-based algorithms like token buckets where permits replenish at a fixed rate.

**Appears as:** in-flight upload limiters, memory budget for buffers, disk I/O throttling.

**Interview signal:** "operations consume different amounts of X" → semaphore where 1 permit = 1 unit of resource.

---

### 3. Reuse Expensive Objects

Fixed set of costly objects (DB connections, GPU contexts) that can't be created on demand. Threads need the actual object, not just permission.

**Use:** blocking queue — threads `get()`, use, `put()` back in `finally`.

```python
class GPUScheduler:
    def __init__(self, gpu_count: int):
        self._pool = queue.Queue(maxsize=gpu_count)
        for i in range(gpu_count):
            self._pool.put(GPU(id=i))

    def run_inference(self, model_input):
        gpu = self._pool.get(timeout=5.0)
        try:
            return gpu.infer(model_input)
        finally:
            self._pool.put(gpu)
```

**Appears as:** database connection pools, GPU schedulers, thread pools, OS file handle pools.

**Interview signal:** "creating X is expensive" or "limited number of X" → blocking queue of objects.

---

### 4. Maximize Utilization

Given limited resources, keep them as busy as possible. Relevant when the interviewer pushes on throughput.

| Technique | What it does | When to use |
|---|---|---|
| **Work stealing** | Each worker has its own queue; idle workers steal from busy ones | Many short tasks mixed with long ones — prevents idle workers |
| **Batching** | Acquire one resource, do N operations, release | Many small operations — amortizes pool overhead, trades latency for throughput |
| **Adaptive sizing** | Pool grows under load, shrinks when idle | Variable traffic patterns — avoids waste during quiet periods |

> [!tip] Interview signal
> "How would you keep the GPUs fully utilized?" → mention work stealing for uneven task lengths, batching for many small operations.

---

## Decision Tree

```
Scarcity problem?
├─ Need to cap concurrent operations (no resource objects)?
│   └─ Semaphore with N permits
├─ Operations consume variable amounts of a shared budget?
│   └─ Semaphore where 1 permit = 1 resource unit
├─ Need to reuse stateful, expensive objects?
│   └─ Blocking queue of objects; get() with timeout, put() in finally
└─ Maximize throughput with limited resources?
    └─ Work stealing / batching / adaptive pool sizing
```

---

## Common Bugs

| Bug | Effect | Fix |
|---|---|---|
| Forget to release in `finally` | Permits/connections leak permanently → pool drains | Always use `with` or `try/finally` |
| Unbounded queue | OOM under load | Always pass `maxsize` |
| `get()` without timeout on request path | Threads block forever → user timeouts | Use `get(timeout=N)` and raise on `Empty` |
| Check-then-act without lock | Threads exceed limits | See [[Concurrency - Correctness]] |

---

## Related
- [[> Concurrency]]
- [[Concurrency - Correctness]]
- [[Concurrency - Coordination]]
