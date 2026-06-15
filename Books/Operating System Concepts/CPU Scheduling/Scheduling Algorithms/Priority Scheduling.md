---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Priority Scheduling Algorithm"]
---

# Priority Scheduling

1) The [[Shortest-Job-First Scheduling|SJF]] algorithm is a special case of the general **priority scheduling algorithm**. A priority is associated with each process, and the CPU is allocated to the process with the highest priority. 
2) Equal-priority processes are scheduled in [[First-Come, First-Served Scheduling|FCFS]] order. An SJF algorithm is simply a priority algorithm where the priority (_p_) is the inverse of the (predicted) next CPU burst. The larger the CPU burst, the lower the priority, and vice versa.
3) Note that we discuss scheduling in terms of _high_ priority and _low_ priority. Priorities are generally indicated by some fixed range of numbers, such as 0 to 7 or 0 to 4,095. 
4) However, there is no general agreement on whether 0 is the highest or lowest priority. Some systems use low numbers to represent low priority; others use low numbers for high priority. This difference can lead to confusion. In this text, we assume that low numbers represent high priority.
5) As an example, consider the following set of processes, assumed to have arrived at time 0 in the order _P_1, _P_2, ···, _P_5, with the length of the CPU burst given in milliseconds:

![[Pasted image 20260609104455.png]]

6) Using priority scheduling, we would schedule these processes according to the following Gantt chart: The average waiting time is 8.2 milliseconds.

![[Pasted image 20260609104507.png]]

7) Priorities can be defined either internally or externally. Internally defined priorities use some measurable quantity or quantities to compute the priority of a process. For example, time limits, memory requirements, the number of open files, and the ratio of average I/O burst to average CPU burst have been used in computing priorities. External priorities are set by criteria outside the operating system, such as the importance of the process, the type and amount of funds being paid for computer use, the department sponsoring the work, and other, often political, factors.
8) Priority scheduling can be either preemptive or nonpreemptive. When a process arrives at the ready queue, its priority is compared with the priority of the currently running process.
9) A preemptive priority scheduling algorithm will preempt the CPU if the priority of the newly arrived process is higher than the priority of the currently running process. A nonpreemptive priority scheduling algorithm will simply put the new process at the head of the ready queue.
10) A major problem with priority scheduling algorithms is **indefinite blocking** , or **starvation**. A process that is ready to run but waiting for the CPU can be considered blocked. A priority scheduling algorithm can leave some low-priority processes waiting indefinitely. 
11) In a heavily loaded computer system, a steady stream of higher-priority processes can prevent a low-priority process from ever getting the CPU. Generally, one of two things will happen. 
12) Either the process will eventually be run (at 2 A.M. Sunday, when the system is finally lightly loaded), or the computer system will eventually crash and lose all unfinished low-priority processes. (Rumor has it that, when they shut down the IBM 7094 at MIT in 1973, they found a low-priority process that had been submitted in 1967 and had not yet been run.)
13) A solution to the problem of indefinite blockage of low-priority processes is **aging**. Aging is a technique of gradually increasing the priority of processes that wait in the system for a long time. For example, if priorities range from 127 (low) to 0 (high), we could increase the priority of a waiting process by 1 every 15 minutes. 
14) Eventually, even a process with an initial priority of 127 would have the highest priority in the system and would be executed. In fact, it would take no more than 32 hours for a priority-127 process to age to a priority-0 process.

## Related

- [[> Scheduling Algorithms]] — back to the sub-topic MOC
- [[Shortest-Job-First Scheduling]] — a special case of priority scheduling
- [[Multilevel Feedback Queue Scheduling]] — uses aging to prevent starvation
- [[Round-Robin Scheduling]] — the sibling time-sharing algorithm
