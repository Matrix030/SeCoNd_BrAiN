---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["Threads Summary"]
---

# Threads Summary

1) A thread is a flow of control within a process. A multithreaded process contains several different flows of control within the same address space.
2) The benefits of multithreading include increased responsiveness to the user, resource sharing within the process, economy, and scalability issues such as more efficient use of multiple cores.
3) User-level threads are threads that are visible to the programmer and are unknown to the kernel. The operating-system kernel supports and manages kernel-level threads.
4) Three different types of models relate user and kernel threads:
	- The many-to-one model maps many user threads to a single kernel thread.
	- The one-to-one model maps each user thread to a corresponding kernel thread.
	- The many-to-many model multiplexes many user threads to a smaller or equal number of kernel threads.
5) Thread libraries provide the application programmer with an API for creating and managing threads. Three primary thread libraries are in common use: POSIX Pthreads, Win32 threads for Windows systems, and Java threads.
6) Multithreaded programs introduce many challenges for the programmer, including the semantics of the fork() and exec() system calls. Other issues include thread cancellation, signal handling, and thread-specific data.
## Related

- [[> Threads]] — back to the chapter MOC
- [[> Overview]] — motivation and benefits of multithreading
- [[> Multithreading Models]] — user and kernel thread models
- [[> Thread Libraries]] — Pthreads, Win32, and Java thread APIs
- [[> Threading Issues]] — fork/exec, cancellation, signals, thread pools
