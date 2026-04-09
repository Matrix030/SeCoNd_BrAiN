---
tags: [concurrency, lld, threads, correctness, locking]
aliases: [Correctness, Concurrency Correctness]
---

# Concurrency - Correctness

Preventing data corruption when multiple threads access shared state.

> [!warning] The danger
> Not deadlock or performance — silently producing **wrong results**. Two threads both book the same seat. A counter that should be 1000 reads 847. A bank balance missing deposits.

---

## The Core Problem

The **check and the act** are two separate steps. Another thread can sneak in between them and invalidate the assumption you checked.

```
Alice checks: seat 7A available? → Yes
Bob  checks: seat 7A available? → Yes   ← sneaks in before Alice acts
Alice books seat 7A
Bob  books seat 7A   ← overwrites Alice
```

The same shape appears everywhere: rate limiters, connection pools, caches — any time a check's validity can change before you act on it.

---

## The Four Solutions

### 1. Coarse-Grained Locking

One lock guards all related state. Default choice for shared state.

```python
import threading

class TicketBooking:
    def __init__(self):
        self._lock = threading.Lock()
        self._seat_owners = {}

    def book_seat(self, seat_id: str, visitor_id: str) -> bool:
        with self._lock:
            if seat_id in self._seat_owners:
                return False
            self._seat_owners[seat_id] = visitor_id
            return True
```

The `with self._lock:` block is a critical section — only one thread at a time. The check and the update happen together with no opportunity for interleaving.

> [!warning] Common mistakes
> - Releasing the lock between check and update (breaks atomicity)
> - Using different lock objects for operations that must be atomic together
>
> **Rule:** all operations that maintain an invariant must use the **same** lock.

> [!tip] When to use
> A human triggers the operation? Coarse-grained locking is almost always enough. Even 10,000 concurrent users won't hit the lock at the exact same microsecond at a rate a mutex can't handle.

**Tradeoff:** Bob waits even if he's booking 12B while Alice books 7A — different seats, no real conflict. Under machine-generated traffic at scale, this serialization becomes a bottleneck.

#### Read-Write Locks

When reads vastly outnumber writes, use a read-write lock. Multiple readers can hold it simultaneously; writers get exclusive access.

> [!tip] Interview signal
> "If reads dominate and writes are rare, I'd use a read-write lock so readers don't block each other. If the ratio is close to 50/50, a simple mutex is usually faster."

---

### 2. Fine-Grained Locking

One lock per resource. Threads only block when competing for the **same** resource.

```python
import threading

class TicketBookingFineGrained:
    def __init__(self):
        self._locks_lock = threading.Lock()
        self._seat_locks = {}
        self._seat_owners = {}

    def _get_lock(self, seat_id: str) -> threading.Lock:
        with self._locks_lock:
            if seat_id not in self._seat_locks:
                self._seat_locks[seat_id] = threading.Lock()
            return self._seat_locks[seat_id]

    def book_seat(self, seat_id: str, visitor_id: str) -> bool:
        with self._get_lock(seat_id):
            if seat_id in self._seat_owners:
                return False
            self._seat_owners[seat_id] = visitor_id
            return True
```

Alice and Bob can book different seats simultaneously. Only same-seat conflicts actually block.

> [!warning] Deadlock risk
> When acquiring multiple locks, always acquire them in a **consistent order**. Otherwise Thread A holds lock-1 waiting for lock-2 while Thread B holds lock-2 waiting for lock-1 — neither can proceed.
>
> ```python
> # Fix: sort seat IDs, always acquire the "smaller" one first
> first = seat1 if seat1 < seat2 else seat2
> second = seat2 if seat1 < seat2 else seat1
> with self._get_lock(first):
>     with self._get_lock(second):
>         # safe swap
> ```

> [!tip] When to use
> Machine-generated traffic at scale — connection pools handling thousands of DB queries/sec, caches serving tens of thousands of requests/sec. Not worth the complexity for human-triggered operations.

---

### 3. Atomic Variables

Special CPU instructions that perform read-modify-write in one uninterruptible step — no lock needed.

```python
import threading

class BookingStats:
    def __init__(self):
        self._lock = threading.Lock()
        self._booked_count = 0

    def on_seat_booked(self):
        with self._lock:
            self._booked_count += 1  # simulated atomic in Python
```

> [!warning] Python limitation
> Python has no native atomics — use `threading.Lock`. For true lock-free atomics, need `ctypes` or a third-party library.

**When atomics work:** single variables — counters, flags, statistics where each update is independent.

**When they don't:** the moment you need two variables to stay in sync. You can't atomically update two separate objects — use a lock instead.

> [!tip] Interview heuristic
> Atomics are great for statistics. The moment you're enforcing a **business rule** (booking, rate limiting, balance), you're usually back to locks.

---

### 4. Thread Confinement (Shared Nothing)

Partition data so each thread owns its slice. No sharing → no race conditions → no locks needed.

Example: Thread 1 handles sections A–M, Thread 2 handles N–Z. Each has its own private seat map.

Used in production by Dragonfly (partitions keyspace across threads) and actor systems like Akka.

> [!warning] Tradeoffs
> - Operations spanning partitions still require coordination
> - Load imbalance if some partitions are hotter
> - Strict enforcement required — accidentally crossing partitions reintroduces all race conditions

> [!tip] Interview signal
> "If we're hitting lock contention limits, we could partition the data and assign each partition to a dedicated thread." Mention it when the interviewer pushes hard on scalability.

---

## The Two Bug Patterns

### Check-Then-Act

Read a condition → decide → act on it. Bug: another thread changes the condition between your read and your action.

```python
# BROKEN: Thread A reads count=99, Thread B reads count=99,
# both pass the check, both increment to 100 — user made 101 requests
def allow_request(self, user_id: str) -> bool:
    count = self._request_counts.get(user_id, 0)
    if count < self._max_requests:
        self._request_counts[user_id] = count + 1
        return True
    return False

# FIXED: lock the check and update together
def allow_request(self, user_id: str) -> bool:
    with self._lock:
        count = self._request_counts.get(user_id, 0)
        if count < self._max_requests:
            self._request_counts[user_id] = count + 1
            return True
        return False
```

**Appears in:** ticket booking, rate limiters, connection pools, LRU caches, parking lots, file download managers.

**Ask yourself:** "Could another thread change this between when I check it and when I act on it?" If yes → wrap both in a lock.

---

### Read-Modify-Write

Read a value → compute from it → write it back. Bug: two threads read the same value, both compute, both write — one update is lost.

```python
# BROKEN: Thread A reads 5, Thread B reads 5, both write 6 — lost increment
self._count += 1

# FIXED
with self._lock:
    self._count += 1
```

**Appears in:** hit counters, bank accounts, metrics aggregators, inventory systems.

**Ask yourself:** "What happens if two threads do this at the same time?" If the answer is "one update gets lost" → synchronize.

---

## Decision Tree

```
Shared mutable state?
├─ Single variable (counter, flag)?
│   └─ Use atomic (or lock in Python)
├─ Multiple fields / business invariant?
│   ├─ Human-triggered, moderate load?
│   │   └─ Coarse-grained locking
│   ├─ High-throughput, machine traffic?
│   │   └─ Fine-grained locking (per-resource locks)
│   └─ Can partition data by owner?
│       └─ Thread confinement
```

---

## Related
- [[> Concurrency]]
- [[Concurrency - Coordination]]
- [[Concurrency - Scarcity]]
