---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.2.1"]
---

# Many-to-One Model

![[Pasted image 20260607110941.png]]

1) The many-to-one model maps many user-level threads to one kernel thread. Thread management is done by the thread library in user space, so it is efficient; but the entire process will block if a thread makes a blocking system call.
2) Also, because only one thread can access the kernel at a time, multiple threads are unable to run in parallel on multiprocessors.

## Related

- [[> Multithreading Models]] — back to the sub-topic MOC
- [[User and Kernel Threads]] — the section preamble
- [[One-to-One Model]] — the next model
- [[Multiprocessor Systems]] — where this model can't run threads in parallel
