---
tags: [book, os, operating-systems, book-os-concepts, concurrency, moc]
aliases: ["Semaphores"]
---

# Semaphores — Map of Content

> [!info] Section 6.5
> Drop raw notes for this sub-topic's sub-sub-topics here. Once filled, this file is processed into leaf notes and rewritten as the index.

The hardware-based solutions to the critical-section problem presented in Section 6.4 are complicated for application programmers to use. To overcome this difficulty, we can use a synchronization tool called a **semaphore**.

1) A semaphore S is an integer variable that, apart from initialization, is accessed only through two standard atomic operations: wait() and signal(). 
	1) The wait() operation was originally termed P (from the Dutch _proberen,_ “to test”); 
	2) signal() was originally called V (from _verhogen,_ “to increment”). 
	3) The definition of wait() is as follows:

	![[Pasted image 20260612125923.png]]

	4) The definition of signal() is as follows:
	![[Pasted image 20260612125932.png]]

2) All modifications to the integer value of the semaphore in the wait() and signal() operations must be executed indivisibly. That is, when one process modifies the semaphore value, no other process can simultaneously modify that same semaphore value. In addition, in the case of wait(S), the testing of the integer value of S (S ≤ 0), as well as its possible modification (S--), must be executed without interruption. We shall see how these operations can be implemented in Section 6.5.2; first, let us see how semaphores can be used.

## **6.5.1 Usage**
1) Operating systems often distinguish between counting and binary semaphores. The value of a **counting semaphore** can range over an unrestricted domain. The value of a **binary semaphore** can range only between 0 and 1. On some systems, binary semaphores are known as **mutex locks**, as they are locks that provide _mutual exclusion_.
2) We can use binary semaphores to deal with the critical-section problem for multiple processes. The _n_ processes share a semaphore, mutex, initialized to 1. Each process _P__i_ is organized as shown in Figure 6.9.

![[Pasted image 20260612125944.png]]

3) Counting semaphores can be used to control access to a given resource consisting of a finite number of instances. The semaphore is initialized to the number of resources available. Each process that wishes to use a resource performs a wait() operation on the semaphore (thereby decrementing the count). When a process releases a resource, it performs a signal() operation (incrementing the count).
4) When the count for the semaphore goes to 0, all resources are being used. After that, processes that wish to use a resource will block until the count becomes greater than 0.
5) We can also use semaphores to solve various synchronization problems. For example, consider two concurrently running processes: _P_1 with a statement _S_1 and _P_2 with a statement _S_2. Suppose we require that _S_2 be executed only after _S_1 has completed. We can implement this scheme readily by letting _P_1 and _P_2 share a common semaphore synch, initialized to 0, and by inserting the statements

![[Pasted image 20260612125954.png]]

in process _P_1 and the statements

![[Pasted image 20260612130002.png]]
in process _P_2. Because synch is initialized to 0, _P_2 will execute _S_2 only after _P_1 has invoked signal(synch), which is after statement _S_1 has been executed.

## **6.5.2 Implementation**
1) The main disadvantage of the semaphore definition given here is that it requires **busy waiting**. While a process is in its critical section, any other process that tries to enter its critical section must loop continuously in the entry code. This continual looping is clearly a problem in a real multiprogramming system, where a single CPU is shared among many processes. 
2) Busy waiting wastes CPU cycles that some other process might be able to use productively. This type of semaphore is also called a **spinlock** because the process “spins” while waiting for the lock. (Spinlocks do have an advantage in that no context switch is required when a process must wait on a lock, and a context switch may take considerable time. Thus, when locks are expected to be held for short times, spinlocks are useful; they are often employed on multiprocessor systems where one thread can “spin” on one processor while another thread performs its critical section on another processor.)

To overcome the need for busy waiting, we can modify the definition of the wait() and signal() semaphore operations. When a process executes the wait() operation and finds that the semaphore value is not positive, it must wait. However, rather than engaging in busy waiting, the process can _block_ itself. The block operation places a process into a waiting queue associated with the semaphore, and the state of the process is switched to the waiting state. Then control is transferred to the CPU scheduler, which selects another process to execute.

A process that is blocked, waiting on a semaphore S, should be restarted when some other process executes a signal() operation. The process is restarted by a wakeup() operation, which changes the process from the waiting state to the ready state. The process is then placed in the ready queue. (The CPU may or may not be switched from the running process to the newly ready process, depending on the CPU-scheduling algorithm.)

To implement semaphores under this definition, we define a semaphore as a “C” struct:

![[Pasted image 20260612130013.png]]

Each semaphore has an integer value and a list of processes list. When a process must wait on a semaphore, it is added to the list of processes. A signal() operation removes one process from the list of waiting processes and awakens that process.

The wait() semaphore operation can now be defined as

![[Pasted image 20260612130025.png]]

The signal() semaphore operation can now be defined as

![[Pasted image 20260612130033.png]]

The block() operation suspends the process that invokes it. The wakeup(P) operation resumes the execution of a blocked process P. These two operations are provided by the operating system as basic system calls.

Note that in this implementation, semaphore values may be negative, although semaphore values are never negative under the classical definition of semaphores with busy waiting. If a semaphore value is negative, its magnitude is the number of processes waiting on that semaphore. This fact results from switching the order of the decrement and the test in the implementation of the wait() operation.

The list of waiting processes can be easily implemented by a link field in each process control block (PCB). Each semaphore contains an integer value and a pointer to a list of PCBs. One way to add and remove processes from the list so as to ensure bounded waiting is to use a FIFO queue, where the semaphore contains both head and tail pointers to the queue. In general, however, the list can use _any_ queueing strategy. Correct usage of semaphores does not depend on a particular queueing strategy for the semaphore lists.

It is critical that semaphores be executed atomically. We must guarantee that no two processes can execute wait() and signal() operations on the same semaphore at the same time. This is a critical-section problem; and in a single-processor environment (that is, where only one CPU exists), we can solve it by simply inhibiting interrupts during the time the wait() and signal() operations are executing. This scheme works in a single-processor environment because, once interrupts are inhibited, instructions from different processes cannot be interleaved. Only the currently running process executes until interrupts are reenabled and the scheduler can regain control.

In a multiprocessor environment, interrupts must be disabled on every processor; otherwise, instructions from different processes (running on different processors) may be interleaved in some arbitrary way. Disabling interrupts on every processor can be a difficult task and furthermore can seriously diminish performance. Therefore, SMP systems must provide alternative locking techniques—such as spinlocks—to ensure that wait() and signal() are performed atomically.

It is important to admit that we have not completely eliminated busy waiting with this definition of the wait() and signal() operations. Rather, we have moved busy waiting from the entry section to the critical sections of application programs. Furthermore, we have limited busy waiting to the critical sections of the wait() and signal() operations, and these sections are short (if properly coded, they should be no more than about ten instructions). Thus, the critical section is almost never occupied, and busy waiting occurs rarely, and then for only a short time. An entirely different situation exists with application programs whose critical sections may be long (minutes or even hours) or may almost always be occupied. In such cases, busy waiting is extremely inefficient.

## **6.5.3 Deadlocks and Starvation**

The implementation of a semaphore with a waiting queue may result in a situation where two or more processes are waiting indefinitely for an event that can be caused only by one of the waiting processes. The event in question is the execution of a signal() operation. When such a state is reached, these processes are said to be **deadlocked**.

To illustrate this, we consider a system consisting of two processes, _P_0 and _P_1, each accessing two semaphores, S and Q, set to the value 1:

![[Pasted image 20260612130044.png]]

Suppose that _P_0 executes wait(S) and then _P_1 executes wait(Q). When _P_0 executes wait(Q), it must wait until _P_1 executes signal(Q). Similarly, when _P_1 executes wait(S), it must wait until _P_0 executes signal(S). Since these signal() operations cannot be executed, _P_0 and _P_1 are deadlocked.

We say that a set of processes is in a deadlock state when every process in the set is waiting for an event that can be caused only by another process in the set. The events with which we are mainly concerned here are _resource acquisition and release._ However, other types of events may result in deadlocks, as we show in Chapter 7. In that chapter, we describe various mechanisms for dealing with the deadlock problem.

Another problem related to deadlocks is **indefinite blocking**, or **starvation** , a situation in which processes wait indefinitely within the semaphore. Indefinite blocking may occur if we remove processes from the list associated with a semaphore in LIFO (last-in, first-out) order.

## **6.5.4 Priority Inversion**

A scheduling challenge arises when a higher-priority process needs to read or modify kernel data that are currently being accessed by a lower-priority process—or a chain of lower-priority processes. Since kernel data are typically protected with a lock, the higher-priority process will have to wait for a lower-priority one to finish with the resource. The situation becomes more complicated if the lower-priority process is preempted in favor of another process with a higher priority. As an example, assume we have three processes, _L_, _M_, and _H_, whose priorities follow the order _L < M < H_. Assume that process _H_ requires resource _R_, which is currently being accessed by process _L_. Ordinarily, process _H_ would wait for _L_ to finish using resource _R_. However, now suppose that process _M_ becomes runnable, thereby preempting process _L_. Indirectly, a process with a lower priority—process _M_—has affected how long process _H_ must wait for _L_ to relinquish resource _R_.

**PRIORITY INVERSION AND THE MARS PATHFINDER**

Priority inversion can be more than a scheduling inconvenience. On systems with tight time constraints (such as real-time systems—see Chapter 19), priority inversion can cause a process to take longer than it should to accomplish a task. When that happens, other failures can cascade, resulting in system failure.

Consider the Mars Pathfinder, a NASA space probe that landed a robot, the Sojourner rover, on Mars in 1997 to conduct experiments. Shortly after the Sojourner began operating, it started to experience frequent computer resets. Each reset reinitialized all hardware and software, including communications. If the problem had not been solved, the Sojourner would have failed in its mission.

The problem was caused by the fact that one high-priority task, “bc_dist,” was taking longer than expected to complete its work. This task was being forced to wait for a shared resource that was held by the lower-priority “ASI/MET” task, which in turn was preempted by multiple medium-priority tasks. The “bc_dist” task would stall waiting for the shared resource, and ultimately the “bc_sched” task would discover the problem and perform the reset. The Sojourner was suffering from a typical case of priority inversion.

The operating system on the Sojourner was VxWorks (see Section 19.6), which had a global variable to enable priority inheritance on all semaphores. After testing, the variable was set on the Sojourner (on Mars!), and the problem was solved.

A full description of the problem, its detection, and its solution was written by the software team lead and is available at [research.microsoft.com/mbj/Mars_Pathfinder/Authoritative_Account.html](http://research.microsoft.com/mbj/Mars_Pathfinder/Authoritative_Account.html).

This problem is known as **priority inversion**. It occurs only in systems with more than two priorities, so one solution is to have only two priorities. That is insufficient for most general-purpose operating systems, however. Typically these systems solve the problem by implementing a **priority-inheritance protocol**. According to this protocol, all processes that are accessing resources needed by a higher-priority process inherit the higher priority until they are finished with the resources in question. When they are finished, their priorities revert to their original values. In the example above, a priority-inheritance protocol would allow process _L_ to temporarily inherit the priority of process _H_, thereby preventing process _M_ from preempting its execution. When process _L_ had finished using resource _R_, it would relinquish its inherited priority from _H_ and assume its original priority. Because resource _R_ would now be available, process _H_—not _M_—would run next.



---

## Related

- [[> Process Synchronization]] — back to the chapter MOC
