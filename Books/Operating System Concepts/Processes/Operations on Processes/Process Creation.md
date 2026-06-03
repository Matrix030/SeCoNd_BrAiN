---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.3.1"]
---

# Process Creation

1) A process may create several new processes, via a create-process system call, during the course of execution. The creating process is called a **parent** process, and the new processes are called the **children** of that process. Each of these new processes may in turn create other processes, forming a **tree** of processes.
2) Most operating systems (including UNIX and the Windows family of operating systems) identify processes according to a unique **process identifier** (or **pid**), which is typically an integer number.
3) The figure below illustrates a typical process tree for the Solaris operating system, showing the name of each process and its pid. In Solaris, the process at the top of the tree is the sched process, with pid of 0. 
4) The sched process creates several children processes—including pageout and fsflush. These processes are responsible for managing memory and file systems. 
5) The sched process also creates the init process, which serves as the root parent process for all user processes. In the figure below, we see two children of init—inetd and dtlogin. inetd is responsible for networking services such as telnet and ftp; dtlogin is the process representing a user login screen. When a user logs in, dtlogin creates an X-windows session (Xsession), which in turns creates the sdt_shel process. 
6) Below sdt_shel, a user’s command-line shell—the C-shell or csh—is created. 
7) In this command-line interface, the user can then invoke various child processes, such as the ls and cat commands. We also see a csh process with pid of 7778 representing a user who has logged onto the system using telnet. This user has started the Netscape browser (pid of 7785) and the emacs editor (pid of 8105).
![[Pasted image 20260602134936.png]]

8) On UNIX, we can obtain a listing of processes by using the ps command. For example, the command ps -el will list complete information for all processes currently active in the system. It is easy to construct a process tree similar to what is shown in the figure above by recursively tracing parent processes all the way to the init process.
9) In general, a process will need certain resources (CPU time, memory, files, I/O devices) to accomplish its task. When a process creates a subprocess, that subprocess may be able to obtain its resources directly from the operating system, or it may be constrained to a subset of the resources of the parent process. 
10) The parent may have to partition its resources among its children, or it may be able to share some resources (such as memory or files) among several of its children. 
11) Restricting a child process to a subset of the parent’s resources prevents any process from overloading the system by creating too many subprocesses.
12) In addition to the various physical and logical resources that a process obtains when it is created, initialization data (input) may be passed along by the parent process to the child process.
13) When a process creates a new process, two possibilities exist in terms of execution:
	1. The parent continues to execute concurrently with its children.
	2. The parent waits until some or all of its children have terminated.
14) There are also two possibilities in terms of the address space of the new process:
	1. The child process is a duplicate of the parent process (it has the same program and data as the parent).
	2. The child process has a new program loaded into it.

15) To illustrate these differences, let’s first consider the UNIX operating system. In UNIX, as we’ve seen, each process is identified by its process identifier, which is a unique integer. A new process is created by the fork() system call. The new process consists of a copy of the address space of the original process. This mechanism allows the parent process to communicate easily with its child process. Both processes (the parent and the child) continue execution at the instruction after the fork(), with one difference: the return code for the fork() is zero for the new (child) process, whereas the (nonzero) process identifier of the child is returned to the parent.
16) Typically, the exec() system call is used after a fork() system call by one of the two processes to replace the process’s memory space with a new program. 
17) The exec() system call loads a binary file into memory (destroying the memory image of the program containing the exec() system call) and starts its execution. 
18) In this manner, the two processes are able to communicate and then go their separate ways. 
19) The parent can then create more children; or, if it has nothing else to do while the child runs, it can issue a wait() system call to move itself off the [[Scheduling Queues|ready queue]] until the termination of the child.
20) The C program shown in the figure below illustrates the UNIX system calls previously described. We now have two different processes running copies of the same program. T
21) he only difference is that the value of pid (the process identifier) for the child process is zero, while that for the parent is an integer value greater than zero (in fact, it is the actual pid of the child process).
22) The child process inherits privileges and scheduling attributes from the parent, as well certain resources, such as open files. 
23) The child process then overlays its address space with the UNIX command /bin/ls (used to get a directory listing) using the execlp() system call (execlp() is a version of the exec() system call). 
24) The parent waits for the child process to complete with the wait() system call. When the child process completes (by either implicitly or explicitly invoking exit()) the parent process resumes from the call to wait(), where it completes using the exit() system call. This is also illustrated in the figure below.

![[Pasted image 20260602135126.png]]

![[Pasted image 20260602135133.png]]

## Related

- [[> Operations on Processes]] — back to the sub-topic MOC
- [[Process Termination]] — how these children eventually end
- [[Scheduling Queues]] — the ready queue a waiting parent is moved off of
- [[The Process]] — the address space duplicated by fork()
