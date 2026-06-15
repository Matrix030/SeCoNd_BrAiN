---
tags: [book, os, operating-systems, book-os-concepts, concurrency]
aliases: ["The Critical-Section Problem", "Critical Section"]
---

# The Critical-Section Problem

1) Consider a system consisting of _n_ processes {_P_0, _P_1, ..., _P__n_-1}. Each process has a segment of code, called a **critical section**, in which the process may be changing common variables, updating a table, writing a file, and so on. 
2) The important feature of the system is that, when one process is executing in its critical section, no other process is to be allowed to execute in its critical section. That is, no two processes are executing in their critical sections at the same time. 
3) The _critical-section problem_ is to design a protocol that the processes can use to cooperate. Each process must request permission to enter its critical section. The section of code implementing this request is the **entry section**. 
4) The critical section may be followed by an **exit section**. The remaining code is the **remainder section**. The general structure of a typical process _P__i_ is shown in the figure below. The entry section and exit section are enclosed in boxes to highlight these important segments of code.
![[Pasted image 20260611165720.png]]

5) A solution to the critical-section problem must satisfy the following three requirements:
	1. **Mutual exclusion**. If process _P__i_ is executing in its critical section, then no other processes can be executing in their critical sections.
	2. **Progress**. If no process is executing in its critical section and some processes wish to enter their critical sections, then only those processes that are not executing in their remainder sections can participate in deciding which will enter its critical section next, and this selection cannot be postponed indefinitely.
	3. **Bounded waiting**. There exists a bound, or limit, on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted.
6) We assume that each process is executing at a nonzero speed. However, we can make no assumption concerning the **relative speed** of the _n_ processes.
7) At a given point in time, many kernel-mode processes may be active in the operating system. As a result, the code implementing an operating system (_kernel code_) is subject to several possible race conditions. 
8) Consider as an example a kernel data structure that maintains a list of all open files in the system. 
9) This list must be modified when a new file is opened or closed (adding the file to the list or removing it from the list). If two processes were to open files simultaneously, the separate updates to this list could result in a race condition. 
10) Other kernel data structures that are prone to possible race conditions include structures for maintaining memory allocation, for maintaining process lists, and for interrupt handling. It is up to kernel developers to ensure that the operating system is free from such race conditions.
11) Two general approaches are used to handle critical sections in operating systems: 
	1) **preemptive kernels** - A preemptive kernel allows a process to be preempted while it is running in kernel mode.
	2) **nonpreemptive kernels** - A nonpreemptive kernel does not allow a process running in kernel mode to be preempted; a kernel-mode process will run until it exits kernel mode, blocks, or voluntarily yields control of the CPU. 
12) Obviously, a nonpreemptive kernel is essentially free from race conditions on kernel data structures, as only one process is active in the kernel at a time. We cannot say the same about preemptive kernels, so they must be carefully designed to ensure that shared kernel data are free from race conditions. 
13) Preemptive kernels are especially difficult to design for SMP architectures, since in these environments it is possible for two kernel-mode processes to run simultaneously on different processors.
14) A preemptive kernel is more suitable for real-time programming, as it will allow a real-time process to preempt a process currently running in the kernel. Furthermore, a preemptive kernel may be more responsive, since there is less risk that a kernel-mode process will run for an arbitrarily long period before relinquishing the processor to waiting processes. Of course, this effect can be minimized by designing kernel code that does not behave in this way. Later in this chapter, we explore how various operating systems manage preemption within the kernel.

## Related

- [[> The Critical-Section Problem]] — back to the sub-topic MOC
- [[> Process Synchronization]] — back to the chapter MOC
- [[Background]] — the race condition this problem guards against
