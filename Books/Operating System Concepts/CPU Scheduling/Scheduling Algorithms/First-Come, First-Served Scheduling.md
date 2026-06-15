---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["FCFS Scheduling", "FCFS"]
---

# First-Come, First-Served Scheduling

1) By far the simplest CPU-scheduling algorithm is the **first-come, first-served (FCFS) scheduling algorithm**. With this scheme, the process that requests the CPU first is allocated the CPU first. The implementation of the FCFS policy is easily managed with a FIFO queue. 
2) When a process enters the ready queue, its PCB is linked onto the tail of the queue. When the CPU is free, it is allocated to the process at the head of the queue. The running process is then removed from the queue. The code for FCFS scheduling is simple to write and understand.
3) On the negative side, the average waiting time under the FCFS policy is often quite long. Consider the following set of processes that arrive at time 0, with the length of the CPU burst given in milliseconds:

![[Pasted image 20260609104302.png]]

4) If the processes arrive in the order _P_1, _P_2, _P_3, and are served in FCFS order, we get the result shown in the following **Gantt chart**, which is a bar chart that illustrates a particular schedule, including the start and finish times of each of the participating processes:

![[Pasted image 20260609104314.png]]

5) The waiting time is 0 milliseconds for process _P_1, 24 milliseconds for process _P_2, and 27 milliseconds for process _P_3. Thus, the average waiting time is (0 + 24 + 27)/3 = 17 milliseconds. If the processes arrive in the order _P_2, _P_3, _P_1, however, the results will be as shown in the following Gantt chart:

![[Pasted image 20260609104325.png]]

6) The average waiting time is now (6 + 0 + 3)/3 = 3 milliseconds. This reduction is substantial. Thus, the average waiting time under an FCFS policy is generally not minimal and may vary substantially if the processes CPU burst times vary greatly.
7) In addition, consider the performance of FCFS scheduling in a dynamic situation. Assume we have one CPU-bound process and many I/O-bound processes. As the processes flow around the system, the following scenario may result. 
8) The CPU-bound process will get and hold the CPU. During this time, all the other processes will finish their I/O and will move into the ready queue, waiting for the CPU. 
9) While the processes wait in the ready queue, the I/O devices are idle. Eventually, the CPU-bound process finishes its CPU burst and moves to an I/O device. 
10) All the I/O-bound processes, which have short CPU bursts, execute quickly and move back to the I/O queues. At this point, the CPU sits idle. The CPU-bound process will then move back to the ready queue and be allocated the CPU. 
11) Again, all the I/O processes end up waiting in the ready queue until the CPU-bound process is done. There is a **convoy effect** as all the other processes wait for the one big process to get off the CPU. 
12) This effect results in lower CPU and device utilization than might be possible if the shorter processes were allowed to go first.
13) Note also that the FCFS scheduling algorithm is nonpreemptive. Once the CPU has been allocated to a process, that process keeps the CPU until it releases the CPU, either by terminating or by requesting I/O. 
14) The FCFS algorithm is thus particularly troublesome for time-sharing systems, where it is important that each user get a share of the CPU at regular intervals. It would be disastrous to allow one process to keep the CPU for an extended period.

## Related

- [[> Scheduling Algorithms]] — back to the sub-topic MOC
- [[Shortest-Job-First Scheduling]] — the sibling algorithm that minimises average waiting time
- [[Round-Robin Scheduling]] — adds preemption to FCFS for time-sharing
- [[Preemptive Scheduling]] — why FCFS being nonpreemptive matters
