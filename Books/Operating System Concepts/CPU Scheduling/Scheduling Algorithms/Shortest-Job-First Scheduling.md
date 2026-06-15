---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["SJF Scheduling", "SJF", "Shortest-Remaining-Time-First Scheduling"]
---

# Shortest-Job-First Scheduling

1) A different approach to CPU scheduling is the **shortest-job-first (SJF) scheduling algorithm**. This algorithm associates with each process the length of the process’s next CPU burst. 
2) When the CPU is available, it is assigned to the process that has the smallest next CPU burst. If the next CPU bursts of two processes are the same, [[First-Come, First-Served Scheduling|FCFS]] scheduling is used to break the tie.
3) As an example of SJF scheduling, consider the following set of processes, with the length of the CPU burst given in milliseconds:

![[Pasted image 20260609104338.png]]

4) Using SJF scheduling, we would schedule these processes according to the following Gantt chart:

![[Pasted image 20260609104348.png]]

5) The waiting time is 3 milliseconds for process _P_1, 16 milliseconds for process _P_2, 9 milliseconds for process _P_3, and 0 milliseconds for process _P_4. Thus, the average waiting time is (3 + 16 + 9 + 0)/4 = 7 milliseconds. By comparison, if we were using the FCFS scheduling scheme, the average waiting time would be 10.25 milliseconds.
6) The SJF scheduling algorithm is provably _optimal,_ in that it gives the minimum average waiting time for a given set of processes. Moving a short process before a long one decreases the waiting time of the short process more than it increases the waiting time of the long process. Consequently, the _average_ waiting time decreases.
7) The real difficulty with the SJF algorithm is knowing the length of the next CPU request. For long-term (job) scheduling in a batch system, we can use as the length the process time limit that a user specifies when he submits the job. 
8) Thus, users are motivated to estimate the process time limit accurately, since a lower value may mean faster response. (Too low a value will cause a time-limit-exceeded error and require resubmission.) SJF scheduling is used frequently in long-term scheduling.
9) Although the SJF algorithm is optimal, it cannot be implemented at the level of short-term CPU scheduling. With short-term scheduling, there is no way to know the length of the next CPU burst. 
10) One approach is to try to approximate SJF scheduling. We may not _know_ the length of the next CPU burst, but we may be able to _predict_ its value. We expect that the next CPU burst will be similar in length to the previous ones. By computing an approximation of the length of the next CPU burst, we can pick the process with the shortest predicted CPU burst.
11) The next CPU burst is generally predicted as an **exponential average** of the measured lengths of previous CPU bursts. We can define the exponential average with the following formula. Let _t__n_ be the length of the _n_th CPU burst, and letτ_n_+1 be our predicted value for the next CPU burst. Then, for α, 0 ≤ α ≤ 1, define

![[Pasted image 20260609104400.png]]

![134](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9780470128725/files/silb_9780470128725_oeb_134_r1.gif)

12) The value of _t__n_ contains our most recent information; τ_n_ stores the past history. The parameter α controls the relative weight of recent and past history in our prediction. 
13) If α = 0, then τ_n_+1 = τ_n_, and recent history has no effect (current conditions are assumed to be transient). If α =1, then τ_n_+1 = _t__n_, and only the most recent CPU burst matters (history is assumed to be old and irrelevant). 
14) More commonly, α = ½, so recent history and past history are equally weighted. The initial τ0 can be defined as a constant or as an overall system average. [Figure 5.3](https://learning.oreilly.com/library/view/operating-system-concepts/9780470128725/silb_9780470128725_oeb_c05_r1.html#FIG-5.3-section-1-6-3-3-2) shows an exponential average with α = ½ and τ0 = 10.

![[Pasted image 20260609104413.png]]

15) To understand the behavior of the exponential average, we can expand the formula for τ_n_+1 by substituting for τ_n_, to find

![[Pasted image 20260609104422.png]]

16) Since both α and (1 - α) are less than or equal to 1, each successive term has less weight than its predecessor.
17) The SJF algorithm can be either preemptive or nonpreemptive. The choice arises when a new process arrives at the ready queue while a previous process is still executing. 
18) The next CPU burst of the newly arrived process may be shorter than what is left of the currently executing process. A preemptive SJF algorithm will preempt the currently executing process, whereas a nonpreemptive SJF algorithm will allow the currently running process to finish its CPU burst. 
19) Preemptive SJF scheduling is sometimes called **shortest-remaining-time-first scheduling**.
20) As an example, consider the following four processes, with the length of the CPU burst given in milliseconds:

![[Pasted image 20260609104435.png]]

21) If the processes arrive at the ready queue at the times shown and need the indicated burst times, then the resulting preemptive SJF schedule is as depicted in the following Gantt chart:

![[Pasted image 20260609104445.png]]

22) Process _P_1 is started at time 0, since it is the only process in the queue. Process _P_2 arrives at time 1. The remaining time for process _P_1 (7 milliseconds) is larger than the time required by process _P_2 (4 milliseconds), so process _P_1 is preempted, and process _P_2 is scheduled. The average waiting time for this example is [(10 - 1) + (1 - 1) + (17 - 2) + (5 - 3)]/4 = 26/4 = 6.5 milliseconds. Nonpreemptive SJF scheduling would result in an average waiting time of 7.75 milliseconds.

## Related

- [[> Scheduling Algorithms]] — back to the sub-topic MOC
- [[First-Come, First-Served Scheduling]] — used to break ties between equal bursts
- [[Priority Scheduling]] — SJF is a special case of priority scheduling
- [[Preemptive Scheduling]] — preemptive SJF is shortest-remaining-time-first
