---
tags: [book, os, operating-systems, book-os-concepts, processes, threads, moc]
aliases: ["Threading Issues"]
---

# Threading Issues — Map of Content

> [!info] Section 4.4
> The semantics and challenges of multithreaded programming — fork()/exec() behaviour, thread cancellation, signal delivery, thread pools, thread-specific data, and scheduler activations.

---

## Notes

- [[The fork() and exec() System Calls]] — how fork()/exec() semantics change with multiple threads
- [[Cancellation]] — terminating a target thread, asynchronously or deferred
- [[Signal Handling]] — delivering synchronous and asynchronous signals in a multithreaded process
- [[Thread Pools]] — pre-creating worker threads to bound thread count and creation cost
- [[Thread-Specific Data]] — giving each thread its own copy of certain data
- [[Scheduler Activations]] — LWPs and upcalls for kernel/thread-library coordination

---

## Related

- [[> Threads]] — back to the chapter MOC
