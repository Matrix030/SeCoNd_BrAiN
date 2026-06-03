---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.3.2"]
---

# Process Termination

1) A process terminates when it finishes executing its final statement and asks the operating system to delete it by using the exit() system call. 
2) At that point, the process may return a status value (typically an integer) to its parent process (via the wait() system call). All the resources of the process—including physical and virtual memory, open files, and I/O buffers—are deallocated by the operating system.
3) Termination can occur in other circumstances as well. A process can cause the termination of another process via an appropriate system call.
4) Usually, such a system call can be invoked only by the parent of the process that is to be terminated. Otherwise, users could arbitrarily kill each other’s jobs. 
5) Note that a parent needs to know the identities of its children. Thus, when one process creates a new process, the identity of the newly created process is passed to the parent.
6) A parent may terminate the execution of one of its children for a variety of reasons, such as these:
	- The child has exceeded its usage of some of the resources that it has been allocated. (To determine whether this has occurred, the parent must have a mechanism to inspect the state of its children.)
	- The task assigned to the child is no longer required.
	- The parent is exiting, and the operating system does not allow a child to continue if its parent terminates.
7) Some systems, including VMS, do not allow a child to exist if its parent has terminated. In such systems, if a process terminates (either normally or abnormally), then all its children must also be terminated. This phenomenon, referred to as **cascading termination**, is normally initiated by the operating system.
8) To illustrate process execution and termination, consider that, in UNIX, we can terminate a process by using the exit() system call; its parent process may wait for the termination of a child process by using the wait() system call. 
9) The wait() system call returns the process identifier of a terminated child so that the parent can tell which of its children has terminated. 
10) If the parent terminates, however, all its children have assigned as their new parent the init process. Thus, the children still have a parent to collect their status and execution statistics.

## Related

- [[> Operations on Processes]] — back to the sub-topic MOC
- [[Process Creation]] — how the children being terminated were created
- [[Process State]] — the terminated state a process reaches
