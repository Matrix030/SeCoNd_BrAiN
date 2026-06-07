---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.2.2"]
---

# One-to-One Model

1) The one-to-one model maps each user thread to a kernel thread. It provides more concurrency than the many-to-one model by allowing another thread to run when a thread makes a blocking system call; it also allows multiple threads to run in parallel on multiprocessors.
2) The only drawback to this model is that creating a user thread requires creating the corresponding kernel thread. Because the overhead of creating kernel threads can burden the performance of an application, most implementations of this model restrict the number of threads supported by the system. Linux, along with the family of Windows operating systems, implement the one-to-one model.

![[Pasted image 20260607110953.png]]

## Related

- [[> Multithreading Models]] — back to the sub-topic MOC
- [[Many-to-One Model]] — the previous model
- [[Many-to-Many Model]] — the next model
- [[Linux]] — one of the systems that implements this model
