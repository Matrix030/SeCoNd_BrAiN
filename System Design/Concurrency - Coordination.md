---
tags: [concurrency, lld, threads, coordination, blocking-queue, actor-model]
aliases: [Coordination, Concurrency Coordination]
---

# Concurrency - Coordination

How threads communicate and hand off work without burning CPU or corrupting state.

> [!tip] The core tension
> Consumers need to sleep when there's no work (not burn CPU polling). Producers need to slow down when consumers can't keep up (not exhaust memory). Both must happen thread-safely.

---

## The Problem Shape

A producer generates work. A consumer processes it. Something coordinates the handoff.

**Naive approaches fail:**

| Approach | Problem |
|---|---|
| Busy-wait (`while True: check`) | Burns 100% CPU doing nothing |
| Sleep-polling (`sleep(0.1)`) | Wastes CPU or adds latency — can't win |
| Unbounded queue | Memory exhaustion under burst load → OOM crash |

The solution needs: **efficient waiting** (sleep until work arrives), **backpressure** (block producers when full), and **thread safety** (no corruption).

---

## Two Paradigms

### 1. Shared State Coordination

Threads communicate via shared data structures. A queue sits between producers and consumers — the queue owns all the synchronization.

#### Wait / Notify (Condition Variables)

The low-level primitive. Rarely used directly in interviews, but explains how blocking queues work internally.

```python
with condition:
    while not condition_is_met():
        condition.wait()   # releases lock, sleeps — zero CPU
    do_work()
    condition.notify_all() # wakes waiting threads
```

- `wait()` atomically releases the lock and parks the thread — no spinning
- `notify_all()` wakes all waiters; they reacquire the lock and recheck the condition
- The `while` loop is mandatory — spurious wakeups exist and another thread may have already consumed what you were waiting for

> [!warning] notify() vs notify_all()
> `notify()` wakes one thread chosen arbitrarily. If producers and consumers share one condition variable, you might wake the wrong type. Use **separate condition variables** — one for "not empty" (consumers wait), one for "not full" (producers wait) — or just use `notify_all()`.

#### Blocking Queue

A thread-safe queue with `put()` and `take()` that block automatically. This is the right default for interviews.

```python
import queue
from dataclasses import dataclass
from typing import Callable

class TaskScheduler:
    def __init__(self):
        self._queue = queue.Queue(maxsize=1000)  # always bound it

    def submit_task(self, task: Callable) -> None:
        self._queue.put(task)   # blocks if full → backpressure

    def worker_loop(self) -> None:
        while True:
            task = self._queue.get()  # blocks if empty → no busy-wait
            task()
```

Built-in backpressure: producers block when the queue fills up. Built-in efficient waiting: consumers sleep when the queue is empty. All synchronization is handled internally.

> [!warning] Always set a capacity
> An unbounded queue (`Queue()` with no `maxsize`) brings back memory exhaustion. Size it based on burst tolerance: if workers process 100 tasks/sec and you want to absorb a 10-second spike → `maxsize=1000`.

**When the queue fills up, choose one:**

| Strategy | How | When |
|---|---|---|
| Block producer | `put()` (default) | Internal pipelines where slowing down is OK |
| Timeout + reject | `put(timeout=0.1)` → raise 503 | Request paths — can't stall the caller |
| Drop + log | `put_nowait()` → catch `Full` | Lossy workloads (analytics, metrics) |

**Graceful shutdown options:**
1. **Interrupt threads** — blocked `get()` raises, worker exits
2. **Poll with timeout** — `get(timeout=1)` returns `None`; worker checks a shutdown flag
3. **Poison pill** — enqueue one sentinel per worker; workers exit when they see it

---

### 2. Message Passing Coordination (Actor Model)

Each entity has a private mailbox and processes messages one at a time. No shared state → no locks within the entity.

```python
import threading
import queue
from abc import ABC, abstractmethod

class Actor(ABC):
    def __init__(self):
        self.mailbox = queue.Queue()
        self.running = True
        self.thread = threading.Thread(target=self._run)
        self.thread.start()

    def _run(self):
        while self.running:
            try:
                message = self.mailbox.get(timeout=0.1)
                self.on_receive(message)
            except queue.Empty:
                continue

    def send(self, message):
        self.mailbox.put(message)

    @abstractmethod
    def on_receive(self, message): pass

    def stop(self):
        self.running = False
        self.thread.join()
```

`on_receive()` never runs concurrently with itself — the actor is single-threaded internally. Any mutable state inside the actor needs no synchronization.

```python
class EmailActor(Actor):
    def __init__(self):
        super().__init__()
        self.email_client = EmailClient()  # owned exclusively

    def on_receive(self, request):
        self.email_client.send(request.to, request.subject, request.body)

# Caller:
email_actor.send(EmailRequest(to=user.email, subject="Welcome!", body="..."))
# returns immediately — actor processes at its own pace
```

> [!tip] When to use actors vs blocking queues
> - **Blocking queue:** "process these tasks in the background" — simple producer-consumer
> - **Actor:** "coordinate many independent entities with their own state" — chat sessions, game rooms, order books, trading systems

> [!warning] Actor tradeoffs
> - Mailbox overflow (same as unbounded queues — bound them)
> - Message ordering: A→B is ordered, but A+C→B is interleaved arbitrarily
> - Request-response is awkward — must build "ask + callback" patterns manually
> - Debugging: tracing bugs across actor message flows is harder than a single call stack

---

## Common Interview Patterns

### Process Requests Asynchronously

Slow work (email, image resize, report generation) doesn't belong on the request path. Enqueue it, respond immediately, process in the background.

```python
class EmailService:
    def __init__(self):
        self._queue = queue.Queue(maxsize=10000)

    def signup(self, email: str, name: str) -> None:
        user_repository.save(email, name)
        self._queue.put(EmailTask(email, "welcome", name))  # instant
        # return success — user doesn't wait for the email

    def email_worker(self) -> None:
        while True:
            task = self._queue.get()
            email_client.send(task.recipient, task.template, task.data)
```

**Same shape:** image upload → resize in background, payment checkout → fulfillment pipeline, report request → async generation with polling.

**Interview signal:** "process this after the request completes" or "this takes too long inline" → blocking queue + worker pool.

---

### Handle Bursty Traffic

Normal load: 100 req/sec. Peak load: 10,000 req/sec for a few minutes. Don't scale workers to peak — use a queue to absorb the spike.

```python
class TicketService:
    def __init__(self):
        self._queue = queue.Queue(maxsize=100000)  # absorbs 10s burst at 10k/s

    def purchase_ticket(self, user_id: str, event_id: str, quantity: int) -> None:
        try:
            self._queue.put(PurchaseRequest(user_id, event_id, quantity), timeout=0.1)
        except queue.Full:
            raise ServiceUnavailableException("Too many requests, try again")

    def purchase_worker(self) -> None:
        while True:
            request = self._queue.get()
            process_purchase(request)  # runs at sustainable rate
```

Workers sized for normal load (say 100 workers). Queue absorbs burst. Workers drain it steadily. Users may wait a few extra seconds but aren't dropped.

**Same shape:** breaking news → analytics pipeline, email campaign clicks, batch job fan-out, webhook bursts.

**Interview signal:** "traffic is unpredictable" or "load comes in spikes" → bounded queue + reject on full.

---

## Decision Tree

```
Need async coordination?
├─ Simple task handoff (background emails, image processing)?
│   └─ Blocking queue + worker pool
├─ Bursty traffic to smooth?
│   └─ Bounded blocking queue; reject with 503 when full
└─ Many stateful entities communicating?
    └─ Actor model
```

---

## Related
- [[> Concurrency]]
- [[Concurrency - Correctness]]
- [[Concurrency - Scarcity]]
