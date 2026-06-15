---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["RR Scheduling", "Round Robin"]
---

# Round-Robin Scheduling

1) The **round-robin (RR) scheduling algorithm** is designed especially for time-sharing systems. It is similar to [[First-Come, First-Served Scheduling|FCFS]] scheduling, but preemption is added to enable the system to switch between processes. 
2) A small unit of time, called a **time quantum** or time slice, is defined. A time quantum is generally from 10 to 100 milliseconds in length. The ready queue is treated as a circular queue.
3) The CPU scheduler goes around the ready queue, allocating the CPU to each process for a time interval of up to 1 time quantum.
4) To implement RR scheduling, we keep the ready queue as a FIFO queue of processes. New processes are added to the tail of the ready queue. The CPU scheduler picks the first process from the ready queue, sets a timer to interrupt after 1 time quantum, and dispatches the process.
5) One of two things will then happen. The process may have a CPU burst of less than 1 time quantum. In this case, the process itself will release the CPU voluntarily. The scheduler will then proceed to the next process in the ready queue. Otherwise, if the CPU burst of the currently running process is longer than 1 time quantum, the timer will go off and will cause an interrupt to the operating system. 
6) A context switch will be executed, and the process will be put at the **tail** of the ready queue. The CPU scheduler will then select the next process in the ready queue.
7) The average waiting time under the RR policy is often long. Consider the following set of processes that arrive at time 0, with the length of the CPU burst given in milliseconds:

![[Pasted image 20260609104523.png]]

8) If we use a time quantum of 4 milliseconds, then process _P_1 gets the first 4 milliseconds. Since it requires another 20 milliseconds, it is preempted after the first time quantum, and the CPU is given to the next process in the queue, process _P_2. Process _P_2 does not need 4 milliseconds, so it quits before its time quantum expires. 
9) The CPU is then given to the next process, process _P_3. Once each process has received 1 time quantum, the CPU is returned to process _P_1 for an additional time quantum. The resulting RR schedule is as follows:

![[Pasted image 20260609104533.png]]

10) Let’s calculate the average waiting time for the above schedule. _P_1 waits for 6 millisconds (10 - 4), _P_2 waits for 4 millisconds, and _P_3 waits for 7 millisconds. Thus, the average waiting time is 17/3 = 5.66 milliseconds.
11) In the RR scheduling algorithm, no process is allocated the CPU for more than 1 time quantum in a row (unless it is the only runnable process). If a process’s CPU burst exceeds 1 time quantum, that process is preempted and is put back in the ready queue. The RR scheduling algorithm is thus preemptive.
12) If there are _n_ processes in the ready queue and the time quantum is _q_, then each process gets 1/_n_ of the CPU time in chunks of at most _q_ time units. Each process must wait no longer than (_n_ - 1) × _q_ time units until its next time quantum. 
13) For example, with five processes and a time quantum of 20 milliseconds, each process will get up to 20 milliseconds every 100 milliseconds.
14) The performance of the RR algorithm depends heavily on the size of the time quantum. At one extreme, if the time quantum is extremely large, the RR policy is the same as the FCFS policy.
15) In contrast, if the time quantum is extremely small (say, 1 millisecond), the RR approach is called **processor sharing** and (in theory) creates the appearance that each of _n_ processes has its own processor running at 1/_n_ the speed of the real processor. This approach was used in Control Data Corporation (CDC) hardware to implement ten peripheral processors with only one set of hardware and ten sets of registers.
16) The hardware executes one instruction for one set of registers, then goes on to the next. This cycle continues, resulting in ten slow processors rather than one fast one. (Actually, since the processor was much faster than memory and each instruction referenced memory, the processors were not much slower than ten real processors would have been.)
17) In software, we need also to consider the effect of context switching on the performance of RR scheduling. Assume, for example, that we have only one process of 10 time units. If the quantum is 12 time units, the process finishes in less than 1 time quantum, with no overhead.
18) If the quantum is 6 time units, however, the process requires 2 quanta, resulting in a context switch. If the time quantum is 1 time unit, then nine context switches will occur, slowing the execution of the process accordingly (see the figure below).
19) Thus, we want the time quantum to be large with respect to the context-switch time. If the context-switch time is approximately 10 percent of the time quantum, then about 10 percent of the CPU time will be spent in context switching. In practice, most modern systems have time quanta ranging from 10 to 100 milliseconds. The time required for a context switch is typically less than 10 microseconds; thus, the context-switch time is a small fraction of the time quantum.

![[Pasted image 20260609104544.png]]

20) Turnaround time also depends on the size of the time quantum. As we can see from [Figure 5.5](https://learning.oreilly.com/library/view/operating-system-concepts/9780470128725/silb_9780470128725_oeb_c05_r1.html#FIG-5.5-section-1-6-3-3-4), the average turnaround time of a set of processes does not necessarily improve as the time-quantum size increases. In general, the average turnaround time can be improved if most processes finish their next CPU burst in a single time quantum. 
21) For example, given three processes of 10 time units each and a quantum of 1 time unit, the average turnaround time is 29. If the time quantum is 10, however, the average turnaround time drops to 20. If context-switch time is added in, the average turnaround time increases even more for a smaller time quantum, since more context switches are required.

![[Pasted image 20260609104556.png]]

22) Although the time quantum should be large compared with the context-switch time, it should not be too large. If the time quantum is too large, RR scheduling degenerates to an FCFS policy. A rule of thumb is that 80 percent of the CPU bursts should be shorter than the time quantum.

## Related

- [[> Scheduling Algorithms]] — back to the sub-topic MOC
- [[First-Come, First-Served Scheduling]] — RR degenerates to FCFS with a large quantum
- [[Preemptive Scheduling]] — RR is inherently preemptive
- [[Multilevel Queue Scheduling]] — often uses RR for the foreground queue
