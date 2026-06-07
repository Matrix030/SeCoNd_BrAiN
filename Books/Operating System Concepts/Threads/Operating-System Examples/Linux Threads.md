---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.5.2"]
---

# Linux Threads

1) Linux provides the fork() system call with the traditional functionality of duplicating a process, as described in Chapter 3. Linux also provides the ability to create threads using the clone() system call. 
2) However, Linux does not distinguish between processes and threads. In fact, Linux generally uses the term _task_—rather than _process_ or _thread_—when referring to a flow of control within a program.
3) When clone() is invoked, it is passed a set of flags, which determine how much sharing is to take place between the parent and child tasks. Some of these flags are listed below:

| flag | meaning |
| --- | --- |
| CLONE_FS | File-system information is shared. |
| CLONE_VM | The same memory space is shared. |
| CLONE_SIGHAND | Signal handlers are shared. |
| CLONE_FILES | The set of open files is shared. |

4) For example, if clone() is passed the flags CLONE_FS, CLONE_VM, CLONE_SIGHAND, and CLONE_FILES, the parent and child tasks will share the same file-system information (such as the current working directory), the same memory space, the same signal handlers, and the same set of open files. 
5) Using clone() in this fashion is equivalent to creating a thread as described in this chapter, since the parent task shares most of its resources with its child task. 
6) However, if none of these flags is set when clone() is invoked, no sharing takes place, resulting in functionality similar to that provided by the fork() system call.
7) The varying level of sharing is possible because of the way a task is represented in the Linux kernel. A unique kernel data structure (specifically, struct task_struct) exists for each task in the system.
8) This data structure, instead of storing data for the task, contains pointers to other data structures where these data are stored—for example, data structures that represent the list of open files, signal-handling information, and virtual memory.
9) When fork() is invoked, a new task is created, along with a _copy_ of all the associated data structures of the parent process. A new task is also created when the clone() system call is made. 
10) However, rather than copying all data structures, the new task _points_ to the data structures of the parent task, depending on the set of flags passed to clone().
11) Several distributions of the Linux kernel now include the NPTL thread library. NPTL (which stands for Native POSIX Thread Library) provides a POSIX-COMPLIANT thread model for Linux systems along with several other features, such as better support for SMP systems, as well as taking advantage of NUMA support. 
12) In addition, the start-up cost for creating a thread is lower with NPTL than with traditional Linux threads.
13) Finally, with NPTL, the system has the potential to support hundreds of thousands of threads. Such support becomes more important with the growth of multicore and other SMP systems.

## Related

- [[> Operating-System Examples]] — back to the sub-topic MOC
- [[Windows XP Threads]] — the other example covered in this section
- [[Process Creation]] — the fork() duplication semantics from Chapter 3
- [[Multicore Programming]] — why scaling to many threads matters on SMP systems
