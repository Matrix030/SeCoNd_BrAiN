---
tags: [concurrency, threads, lld, shared-memory]
aliases: [Concurrency Intro, Concurrency Overview]
---

# Concurrency

What happens when multiple things try to happen at the same time — and how to handle it.

> [!tip] When it shows up in interviews
> Not every LLD interview. Common at senior level — either as a follow-up to a classic problem ("now two cars race for the same spot") or as the prompt itself (thread pools, rate limiters, connection pools).

---

## The Core Fact

Threads in the same process **share memory**.

- Each thread has its own stack, registers, and program counter
- All threads share the heap, globals, and open resources
- On multi-core: threads run in parallel. On single-core: OS interleaves instructions. From the program's view — **same thing**

Operations that look atomic in source code are often multiple machine instructions. If two threads read/write shared memory without coordination, the outcome depends on timing. This is why concurrency bugs are nondeterministic and hard to reproduce.

> [!warning] JavaScript/TypeScript exception
> User code runs on a single main thread — concurrency expressed via event loops and async callbacks, not shared-memory threads. No shared-state races.

---

## The Primitives

You don't invent synchronization mechanisms — you pick the right existing one.

| Primitive | What it does | Use for |
|---|---|---|
| **Atomics** | Thread-safe ops on a single variable (CAS under the hood) | Counters, flags, simple stats |
| **Lock (Mutex)** | One thread at a time in a critical section | Protecting shared state |
| **Semaphore** | Counting lock — N permits before blocking | Limiting concurrent operations |
| **Condition Variable** | Sleep until a condition is true (releases lock while waiting) | Foundation for blocking queues |
| **Blocking Queue** | Thread-safe producer/consumer handoff; blocks on full/empty | Handing work between threads |

> [!warning] Python atomics
> Python lacks native atomics — use `threading.Lock` to protect counter increments. The GIL also means CPU-bound threads don't truly run in parallel; use `multiprocessing` for that.

### Language Quick Reference

| | Java | Python | Go | C# |
|---|---|---|---|---|
| Lock | `ReentrantLock` | `threading.Lock` | `sync.Mutex` | `lock` |
| RW Lock | `ReentrantReadWriteLock` | N/A (3rd party) | `sync.RWMutex` | `ReaderWriterLockSlim` |
| Semaphore | `Semaphore` | `threading.Semaphore` | `x/sync/semaphore` | `SemaphoreSlim` |
| Blocking Queue | `LinkedBlockingQueue` | `queue.Queue` | buffered channel | `BlockingCollection` |
| Atomic Int | `AtomicInteger` | N/A (use Lock) | `sync/atomic` | `Interlocked` |

> [!tip] Go
> Channels replace blocking queues and condition variables idiomatically. When in doubt, use a channel.

---

## Three Problem Types

Most interview concurrency problems map to one of these:

| Type | What breaks | Tool |
|---|---|---|
| [[Concurrency - Correctness\|Correctness]] | Shared state gets corrupted | Locks, atomics |
| [[Concurrency - Coordination\|Coordination]] | Threads need to hand off work or wait on each other | Blocking queues |
| [[Concurrency - Scarcity\|Scarcity]] | Resources are limited, requests must wait | Semaphores, resource pools |

> [!tip] Typical progression
> Most questions start with **Correctness**. Coordination and Scarcity appear as follow-ups once shared state exists or throughput increases.

---

## Related
- [[Concurrency - Correctness]]
- [[Concurrency - Coordination]]
- [[Concurrency - Scarcity]]
- [[> LLD Delivery Framework]]
