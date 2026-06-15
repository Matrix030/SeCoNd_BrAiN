---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Multilevel Queue Scheduling Algorithm"]
---

# Multilevel Queue Scheduling

> [!note]
> [TODO - wtf is multilvel queue stuff]

1) Another class of scheduling algorithms has been created for situations in which processes are easily classified into different groups. For example, a common division is made between **foreground** (interactive) processes and **background** (batch) processes. 
2) These two types of processes have different response-time requirements and so may have different scheduling needs. In addition, foreground processes may have priority (externally defined) over background processes.
3) A **multilevel queue scheduling algorithm** partitions the ready queue into several separate queues (see figure beelow). The processes are permanently assigned to one queue, generally based on some property of the process, such as memory size, process priority, or process type. Each queue has its own scheduling algorithm. For example, separate queues might be used for foreground and background processes. 
4) The foreground queue might be scheduled by an [[Round-Robin Scheduling|RR]] algorithm, while the background queue is scheduled by an [[First-Come, First-Served Scheduling|FCFS]] algorithm.

![[Pasted image 20260609104611.png]]

5) In addition, there must be scheduling among the queues, which is commonly implemented as fixed-priority preemptive scheduling. For example, the foreground queue may have absolute priority over the background queue.
6) Let’s look at an example of a multilevel queue scheduling algorithm with five queues, listed below in order of priority:
	1. System processes
	2. Interactive processes
	3. Interactive editing processes
	4. Batch processes
	5. Student processes

7) Each queue has absolute priority over lower-priority queues. No process in the batch queue, for example, could run unless the queues for system processes, interactive processes, and interactive editing processes were all empty. 
8) If an interactive editing process entered the ready queue while a batch process was running, the batch process would be preempted.
9) Another possibility is to time-slice among the queues. Here, each queue gets a certain portion of the CPU time, which it can then schedule among its various processes. For instance, in the foreground-background queue example, the foreground queue can be given 80 percent of the CPU time for RR scheduling among its processes, whereas the background queue receives 20 percent of the CPU to give to its processes on an FCFS basis.

## Related

- [[> Scheduling Algorithms]] — back to the sub-topic MOC
- [[Multilevel Feedback Queue Scheduling]] — the flexible variant that lets processes move between queues
- [[Priority Scheduling]] — scheduling among the queues is fixed-priority
- [[Round-Robin Scheduling]] — commonly used within the foreground queue
