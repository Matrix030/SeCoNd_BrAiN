---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.4.1"]
---

# The fork() and exec() System Calls

1) In Chapter 3, we described how the fork() system call is used to create a separate, duplicate process. The semantics of the fork() and exec() system calls change in a multithreaded program.
2) If one thread in a program calls fork(), does the new process duplicate all threads, or is the new process single-threaded? Some UNIX systems have chosen to have two versions of fork(), one that duplicates all threads and another that duplicates only the thread that invoked the fork() system call.
3) The exec() system call typically works in the same way as described in Chapter 3. That is, if a thread invokes the exec() system call, the program specified in the parameter to exec() will replace the entire process—including all threads.

> [!note] The JVM and the Host Operating System
> 1) The JVM is typically implemented on top of a host operating system. This setup allows the JVM to hide the implementation details of the underlying operating system and to provide a consistent, abstract environment that allows Java programs to operate on any platform that supports a JVM.
> 2) The specification for the JVM does not indicate how Java threads are to be mapped to the underlying operating system, instead leaving that decision to the particular implementation of the JVM. For example, the Windows XP operating system uses the [[One-to-One Model|one-to-one model]]; therefore, each Java thread for a JVM running on such a system maps to a kernel thread. 
> 3) On operating systems that use the [[Many-to-Many Model|many-to-many model]] (such as Tru64 UNIX), a Java thread is mapped according to the many-to-many model. 

4) Which of the two versions of fork() to use depends on the application. If exec() is called immediately after forking, then duplicating all threads is unnecessary, as the program specified in the parameters to exec() will replace the process. In this instance, duplicating only the calling thread is appropriate. If, however, the separate process does not call exec() after forking, the separate process should duplicate all threads.

## Related

- [[> Threading Issues]] — back to the sub-topic MOC
- [[Process Creation]] — the single-threaded fork()/exec() semantics from Chapter 3
- [[Cancellation]] — the next threading issue
- [[Java Threads]] — how the JVM maps Java threads to host threads
