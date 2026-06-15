---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Example: Solaris Scheduling"]
---

# Solaris Scheduling

1) Solaris uses priority-based thread scheduling where each thread belongs to one of six classes:
	1. Time sharing (TS)
	2. Interactive (IA)
	3. Real time (RT)
	4. System (SYS)
	5. Fair share (FSS)
	6. Fixed priority (FP)
2) Within each class there are different priorities and different scheduling algorithms.
3) The default scheduling class for a process is time sharing. The scheduling policy for the time-sharing class dynamically alters priorities and assigns time slices of different lengths using a multilevel feedback queue. By default, there is an inverse relationship between priorities and time slices. The higher the priority, the smaller the time slice; and the lower the priority, the larger the time slice. 
4) Interactive processes typically have a higher priority; CPU-bound processes, a lower priority. This scheduling policy gives good response time for interactive processes and good throughput for CPU-bound processes. The interactive class uses the same scheduling policy as the time-sharing class, but it gives windowing applications—such as those created by the KDE or GNOME window managers—a higher priority for better performance.

![[Pasted image 20260611152341.png]]

5) Figure 5.12 shows the dispatch table for scheduling time-sharing and interactive threads. These two scheduling classes include 60 priority levels, but for brevity, we display only a handful. The dispatch table shown in Figure 5.12 contains the following fields:
	- **Priority**. The class-dependent priority for the time-sharing and interactive classes. A higher number indicates a higher priority.
	- **Time quantum**. The time quantum for the associated priority. This illustrates the inverse relationship between priorities and time quanta: the lowest priority (priority 0) has the highest time quantum (200 milliseconds), and the highest priority (priority 59) has the lowest time quantum (20 milliseconds).
	- **Time quantum expired**. The new priority of a thread that has used its entire time quantum without blocking. Such threads are considered CPU-intensive. As shown in the table, these threads have their priorities lowered.
	- **Return from sleep**. The priority of a thread that is returning from sleeping (such as waiting for I/O). As the table illustrates, when I/O is available for a waiting thread, its priority is boosted to between 50 and 59, thus supporting the scheduling policy of providing good response time for interactive processes.
6) Threads in the real-time class are given the highest priority. This assignment allows a real-time process to have a guaranteed response from the system within a bounded period of time. A real-time process will run before a process in any other class. In general, however, few processes belong to the real-time class.
7) Solaris uses the system class to run kernel threads, such as the scheduler and paging daemon. Once established, the priority of a system thread does not change. The system class is reserved for kernel use (user processes running in kernel mode are not in the system class).
8) The fixed-priority and fair-share classes were introduced with Solaris 9. Threads in the fixed-priority class have the same priority range as those in the time-sharing class; however, their priorities are not dynamically adjusted. The fair-share scheduling class uses CPU **shares** instead of priorities to make scheduling decisions. CPU shares indicate entitlement to available CPU resources and are allocated to a set of processes (known as a **project**).
9) Each scheduling class includes a set of priorities. However, the scheduler converts the class-specific priorities into global priorities and selects the thread with the highest global priority to run. The selected thread runs on the CPU until it (1) blocks, (2) uses its time slice, or (3) is preempted by a higher-priority thread. If there are multiple threads with the same priority, the scheduler uses a round-robin queue. Figure 5.13 illustrates how the six scheduling classes relate to one another and how they map to global priorities. Notice that the kernel maintains 10 threads for servicing interrupts. These threads do not belong to any scheduling class and execute at the highest priority (160-169). As mentioned, Solaris has traditionally used the many-to-many model (Section 4.2.3) but switched to the one-to-one model (Section 4.2.2) beginning with Solaris 9.
