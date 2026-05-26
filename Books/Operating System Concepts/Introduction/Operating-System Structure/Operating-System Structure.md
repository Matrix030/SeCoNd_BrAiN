---
tags: [book, os, operating-systems, book-os-concepts, scheduling, memory]
aliases: ["1.4", "Computer-System Structure"]
---

# Computer-System Structure

1) An operating system provides the environment within which programs are executed. 
2) One of the most important aspects of operating systems is the ability to multiprogram. A single program cannot, in general, keep either the CPU or the I/O devices busy at all times. Single users frequently have multiple programs running. **[[Multiprogramming]]** increases CPU utilization by organizing jobs (code and data) so that the CPU always has one to execute.
3) The idea is as follows: The operating system keeps several jobs in memory simultaneously. Since, in general, main memory is too small to accommodate all jobs, the jobs are kept initially on the disk in the **job pool**. This pool consists of all processes residing on disk awaiting allocation of main memory.
4) The set of jobs in memory can be a subset of the jobs kept in the job pool. The operating system picks and begins to execute one of the jobs in memory. Eventually, the job may have to wait for some task, such as an I/O operation, to complete.
5) In a non-multiprogrammed system, the CPU would sit idle. In a multiprogrammed system, the operating system simply switches to, and executes, another job. When _that_ job needs to wait, the CPU is switched to _another_ job, and so on. Eventually, the first job finishes waiting and gets the CPU back. As long as at least one job needs to execute, the CPU is never idle.
![[Pasted image 20260526170604.png]]

6) This idea is common in other life situations. A lawyer does not work for only one client at a time, for example. While one case is waiting to go to trial or have papers typed, the lawyer can work on another case. If he has enough clients, the lawyer will never be idle for lack of work. (Idle lawyers tend to become politicians, so there is a certain social value in keeping lawyers busy.)
7) Multiprogrammed systems provide an environment in which the various system resources (for example, CPU, memory, and peripheral devices) are utilized effectively, but they do not provide for user interaction with the computer system. **[[Time sharing]]** (or **multitasking**) is a logical extension of multiprogramming. In time-sharing systems, the CPU executes multiple jobs by switching among them, but the switches occur so frequently that the users can interact with each program while it is running.
8) Time sharing requires an **interactive** (or **hands-on**) **computer system**, which provides direct communication between the user and the system. The user gives instructions to the operating system or to a program directly, using a input device such as a keyboard or a mouse, and waits for immediate results on an output device. Accordingly, the response time should be short—typically less than one second.
9) A time-shared operating system allows many users to share the computer simultaneously. Since each action or command in a time-shared system tends to be short, only a little CPU time is needed for each user. As the system switches rapidly from one user to the next, each user is given the impression that the entire computer system is dedicated to his use, even though it is being shared among many users.
10) A time-shared operating system uses [[CPU scheduling]] and multiprogramming to provide each user with a small portion of a time-shared computer. Each user has at least one separate program in memory. A program loaded into memory and executing is called a **[[process]]**.
11) When a [[process]] executes, it typically executes for only a short time before it either finishes or needs to perform I/O. I/O may be interactive; that is, output goes to a display for the user, and input comes from a user keyboard, mouse, or other device. 
12) Since interactive I/O typically runs at "people speeds," it may take a long time to complete. Input, for example, may be bounded by the user's typing speed; seven characters per second is fast for people but incredibly slow for computers. Rather than let the CPU sit idle as this interactive input takes place, the operating system will rapidly switch the CPU to the program of some other user.
13) Time sharing and multiprogramming require that several jobs be kept simultaneously in memory. If several jobs are ready to be brought into memory, and if there is not enough room for all of them, then the system must choose among them. Making this decision is **job scheduling**. 
14) When the operating system selects a job from the job pool, it loads that job into memory for execution. Having several programs in memory at the same time requires some form of memory management. In addition, if several jobs are ready to run at the same time, the system must choose among them. Making this decision is **[[CPU scheduling]]**.
15) Finally, running multiple jobs concurrently requires that their ability to affect one another be limited in all phases of the operating system, including process scheduling, disk storage, and memory management.
16) In a time-sharing system, the operating system must ensure reasonable response time, which is sometimes accomplished through **[[swapping]]**, where processes are swapped in and out of main memory to the disk. A more common method for achieving this goal is **[[virtual memory]]**, a technique that allows the execution of a process that is not completely in memory. 
17) The main advantage of the virtual-memory scheme is that it enables users to run programs that are larger than actual **physical memory**. Further, it abstracts main memory into a large, uniform array of storage, separating **logical memory** as viewed by the user from physical memory. 
18) This arrangement frees programmers from concern over memory-storage limitations.
19) Time-sharing systems must also provide a file system. The file system resides on a collection of disks; hence, disk management must be provided. Also, time-sharing systems provide a mechanism for protecting resources from inappropriate use. To ensure orderly execution, the system must provide mechanisms for job synchronization and communication, and it may ensure that jobs do not get stuck in a **[[deadlock]]**, forever waiting for one another.

## Related

- [[> Operating-System Structure]] — back to the sub-topic MOC
- [[> Computer-System Architecture]] — how systems are organized by processor count
- [[CPU scheduling]] — picking which job runs next
- [[virtual memory]] — running programs larger than physical memory
