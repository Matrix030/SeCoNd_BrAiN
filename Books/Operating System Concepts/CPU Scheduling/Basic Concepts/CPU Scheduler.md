---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["CPU Scheduler", "Short-term Scheduler"]
---

# CPU Scheduler

1) Whenever the CPU becomes idle, the operating system must select one of the processes in the ready queue to be executed. The selection process is carried out by the **short-term scheduler** (or CPU scheduler). 
2) The scheduler selects a process from the processes in memory that are ready to execute and allocates the CPU to that process.
3) Note that the ready queue is not necessarily a first-in, first-out (FIFO) queue. As we shall see when we consider the various scheduling algorithms, a ready queue can be implemented as a FIFO queue, a priority queue, a tree, or simply an unordered linked list. Conceptually, however, all the processes in the ready queue are lined up waiting for a chance to run on the CPU. The records in the queues are generally process control blocks (PCBs) of the processes.

## Related

- [[> Basic Concepts]] — back to the sub-topic MOC
- [[CPU-I/O Burst Cycle]] — the process property the scheduler relies on
- [[Preemptive Scheduling]] — when scheduling decisions are made
- [[Dispatcher]] — hands the CPU to the process the scheduler selects
