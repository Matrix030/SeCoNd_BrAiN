---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Multilevel Feedback Queue Scheduling Algorithm"]
---

# Multilevel Feedback Queue Scheduling

1) Normally, when the [[Multilevel Queue Scheduling|multilevel queue scheduling algorithm]] is used, processes are permanently assigned to a queue when they enter the system. If there are separate queues for foreground and background processes, for example, processes do not move from one queue to the other, since processes do not change their foreground or background nature. This setup has the advantage of low scheduling overhead, but it is inflexible.
2) The **multilevel feedback queue scheduling algorithm**, in contrast, allows a process to move between queues. The idea is to separate processes according to the characteristics of their CPU bursts. If a process uses too much CPU time, it will be moved to a lower-priority queue. This scheme leaves I/O-bound and interactive processes in the higher-priority queues. In addition, a process that waits too long in a lower-priority queue may be moved to a higher-priority queue. This form of [[Priority Scheduling|aging]] prevents starvation.
3) For example, consider a multilevel feedback queue scheduler with three queues, numbered from 0 to 2 ([Figure 5.7](https://learning.oreilly.com/library/view/operating-system-concepts/9780470128725/silb_9780470128725_oeb_c05_r1.html#FIG-5.7-section-1-6-3-3-6)). The scheduler first executes all processes in queue 0. Only when queue 0 is empty will it execute processes in queue 1. Similarly, processes in queue 2 will only be executed if queues 0 and 1 are empty. A process that arrives for queue 1 will preempt a process in queue 2. A process in queue 1 will in turn be preempted by a process arriving for queue 0.
4) A process entering the ready queue is put in queue 0. A process in queue 0 is given a time quantum of 8 milliseconds. If it does not finish within this time, it is moved to the tail of queue 1. If queue 0 is empty, the process at the head of queue 1 is given a quantum of 16 milliseconds. If it does not complete, it is preempted and is put into queue 2. Processes in queue 2 are run on an FCFS basis but are run only when queues 0 and 1 are empty.
5) This scheduling algorithm gives highest priority to any process with a CPU burst of 8 milliseconds or less. Such a process will quickly get the CPU, finish its CPU burst, and go off to its next I/O burst. Processes that need more than 8 but less than 24 milliseconds are also served quickly, although with lower priority than shorter processes. Long processes automatically sink to queue 2 and are served in FCFS order with any CPU cycles left over from queues 0 and 1.

![[Pasted image 20260609104623.png]]

6) In general, a multilevel feedback queue scheduler is defined by the following parameters:
	- The number of queues
	- The scheduling algorithm for each queue
	- The method used to determine when to upgrade a process to a higher-priority queue
	- The method used to determine when to demote a process to a lower-priority queue
	- The method used to determine which queue a process will enter when that process needs service
7) The definition of a multilevel feedback queue scheduler makes it the most general CPU-scheduling algorithm. It can be configured to match a specific system under design. Unfortunately, it is also the most complex algorithm, since defining the best scheduler requires some means by which to select values for all the parameters.

## Related

- [[> Scheduling Algorithms]] — back to the sub-topic MOC
- [[Multilevel Queue Scheduling]] — the rigid variant this one builds on
- [[Priority Scheduling]] — aging here is the same anti-starvation technique
- [[Round-Robin Scheduling]] — the per-queue quanta echo RR time slicing
