---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["1.6"]
---

# Process Management

1) A program does nothing unless its instructions are executed by a CPU.
2) A [[process]] needs certain resources—including CPU time, memory, files, and I/O devices—to accomplish its task. These resources are either given to the process when it is created or allocated to it while it is running.
3) In addition to the various physical and logical resources that a process obtains when it is created, various initialization data (input) may be passed along. For example, consider a process whose function is to display the status of a file on the screen of a terminal. 
4) The process will be given as an input the name of the file and will execute the appropriate instructions and [[system call|system calls]] to obtain and display on the terminal the desired information. When the process terminates, the operating system will reclaim any reusable resources.
5) We emphasize that a program by itself is not a process; a program is a _passive_ entity, like the contents of a file stored on disk, whereas a process is an _active_ entity. A single-threaded process has one **program counter** specifying the next instruction to execute. The execution of such a process must be sequential.
6) The CPU executes one instruction of the process after another, until the process completes. Further, at any time, one instruction at most is executed on behalf of the process.
7) Thus, although two processes may be associated with the same program, they are nevertheless considered two separate execution sequences. A multithreaded process has multiple program counters, each pointing to the next instruction to execute for a given thread.
8) A process is the unit of work in a system. Such a system consists of a collection of processes, some of which are operating-system processes (those that execute system code) and the rest of which are user processes (those that execute user code). 
9) All these processes can potentially execute concurrently—by multiplexing on a single CPU, for example.

The operating system is responsible for the following activities in connection with process management:
- Scheduling processes and threads on the CPUs
- Creating and deleting both user and system processes
- Suspending and resuming processes
- Providing mechanisms for [[process synchronization]]
- Providing mechanisms for [[process communication]]

## Related

- [[> Process Management]] — back to the sub-topic MOC
- [[CPU scheduling]] — how the OS schedules processes on the CPU
- [[Dual-Mode Operation]] — how the OS protects processes from one another
- [[Operating-System Structure]] — where processes, CPU scheduling, and virtual memory are introduced
