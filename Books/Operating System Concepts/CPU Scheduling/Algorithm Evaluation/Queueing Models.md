---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Queueing Models", "Queueing-Network Analysis"]
---

# Queueing Models

1) On many systems, the processes that are run vary from day to day, so there is no static set of processes (or times) to use for deterministic modeling. What can be determined, however, is the distribution of CPU and I/O bursts.
2) These distributions can be measured and then approximated or simply estimated. The result is a mathematical formula describing the probability of a particular CPU burst.
3) Commonly, this distribution is exponential and is described by its mean. Similarly, we can describe the distribution of times when processes arrive in the system (the arrival-time distribution). 
4) From these two distributions, it is possible to compute the average throughput, utilization, waiting time, and so on for most algorithms.
5) The computer system is described as a network of servers. Each server has a queue of waiting processes. The CPU is a server with its ready queue, as is the I/O system with its device queues. 
6) Knowing arrival rates and service rates, we can compute utilization, average queue length, average wait time, and so on. This area of study is called **queueing-network analysis**.
7) As an example, let _n_ be the average queue length (excluding the process being serviced), let _W_ be the average waiting time in the queue, and let λ be the average arrival rate for new processes in the queue (such as three processes per second). We expect that during the time _W_ that a process waits, λ × _W_ new processes will arrive in the queue. If the system is in a steady state, then the number of processes leaving the queue must be equal to the number of processes that arrive. Thus,

![[Pasted image 20260611155024.png]]

8) This equation, known as **Little’s formula**, is particularly useful because it is valid for any scheduling algorithm and arrival distribution.
9) We can use Little’s formula to compute one of the three variables if we know the other two. For example, if we know that 7 processes arrive every second (on average), and that there are normally 14 processes in the queue, then we can compute the average waiting time per process as 2 seconds.
10) Queueing analysis can be useful in comparing scheduling algorithms, but it also has limitations. At the moment, the classes of algorithms and distributions that can be handled are fairly limited. The mathematics of complicated algorithms and distributions can be difficult to work with. 
11) Thus, arrival and service distributions are often defined in mathematically tractable —but unrealistic—ways. It is also generally necessary to make a number of independent assumptions, which may not be accurate.
12) As a result of these difficulties, queueing models are often only approximations of real systems, and the accuracy of the computed results may be questionable.

## Related

- [[> Algorithm Evaluation]] — back to the sub-topic MOC
- [[Deterministic Modeling]] — the analytic method for static workloads
- [[Simulations]] — the next, more accurate evaluation method
