---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["CPU Utilization", "Throughput", "Turnaround Time", "Waiting Time", "Response Time"]
---

# Scheduling Criteria

1) Many criteria have been suggested for comparing CPU-scheduling algorithms. Which characteristics are used for comparison can make a substantial difference in which algorithm is judged to be best. The criteria include the following:
	- **CPU utilization**. 
		- We want to keep the CPU as busy as possible. 
		- Conceptually, CPU utilization can range from 0 to 100 percent.
		- In a real system, it should range from 40 percent (for a lightly loaded system) to 90 percent (for a heavily used system).
	- **Throughput**. 
		- If the CPU is busy executing processes, then work is being done. 
		- One measure of work is the number of processes that are completed per time unit, called _throughput._ 
		- For long processes, this rate may be one process per hour; for short transactions, it may be ten processes per second.
	- **Turnaround time**. 
		- From the point of view of a particular process, the important criterion is how long it takes to execute that process. 
		- The interval from the time of submission of a process to the time of completion is the _turnaround time._ Turnaround time is the sum of the periods spent waiting to get into memory, waiting in the ready queue, executing on the CPU, and doing I/O.
	- **Waiting time**. 
		- The CPU-scheduling algorithm does not affect the amount of time during which a process executes or does I/O; it affects only the amount of time that a process spends waiting in the [[CPU Scheduler|ready queue]].
		- _Waiting time_ is the sum of the periods spent waiting in the ready queue.
	- **Response time**. 
		- In an interactive system, turnaround time may not be the best criterion. 
		- Often, a process can produce some output fairly early and can continue computing new results while previous results are being output to the user. 
		- Thus, another measure is the time from the submission of a request until the first response is produced. This measure, called _response time,_ is the time it takes to start responding, not the time it takes to output the response. The turnaround time is generally limited by the speed of the output device.
2) It is desirable to maximize CPU utilization and throughput and to minimize turnaround time, waiting time, and response time. 
3) In most cases, we optimize the average measure. 
4) However, under some circumstances, it is desirable to optimize the minimum or maximum values rather than the average. For example, to guarantee that all users get good service, we may want to minimize the maximum response time.
5) Investigators have suggested that, for interactive systems (such as time-sharing systems), it is more important to minimize the _variance_ in the response time than to minimize the average response time. A system with reasonable and _predictable_ response time may be considered more desirable than a system that is faster on the average but is highly variable. However, little work has been done on CPU-scheduling algorithms that minimize variance.

## Related

- [[> Scheduling Criteria]] — back to the sub-topic MOC
- [[CPU Scheduler]] — the ready queue these criteria measure against
- [[CPU-I/O Burst Cycle]] — burst behavior that influences these measures
- [[Multiprogramming]] — the goal of maximizing CPU utilization
