---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Deterministic Modeling"]
---

# Deterministic Modeling

1) One major class of evaluation methods is **analytic evaluation**. Analytic evaluation uses the given algorithm and the system workload to produce a formula or number that evaluates the performance of the algorithm for that workload.
2) **Deterministic modeling** is one type of analytic evaluation. This method takes a particular predetermined workload and defines the performance of each algorithm for that workload. 
	1) For example, assume that we have the workload shown below. All five processes arrive at time 0, in the order given, with the length of the CPU burst given in milliseconds:

	![[Pasted image 20260611154941.png]]

	2) Consider the [[First-Come, First-Served Scheduling|FCFS]], [[Shortest-Job-First Scheduling|SJF]], and [[Round-Robin Scheduling|RR]] (quantum = 10 milliseconds) scheduling algorithms for this set of processes. Which algorithm would give the minimum average waiting time?

	For the FCFS algorithm, we would execute the processes as

	![[Pasted image 20260611154949.png]]

	3) The waiting time is 0 milliseconds for process _P_1, 10 milliseconds for process _P_2, 39 milliseconds for process _P_3, 42 milliseconds for process _P_4, and 49 milliseconds for process _P_5. Thus, the average waiting time is (0 + 10 + 39 + 42 + 49)/5 = 28 milliseconds.
	With nonpreemptive SJF scheduling, we execute the processes as
	![[Pasted image 20260611155001.png]]

	4) The waiting time is 10 milliseconds for process _P_1, 32 milliseconds for process _P_2, 0 milliseconds for process _P_3, 3 milliseconds for process _P_4, and 20 milliseconds for process _P_5. Thus, the average waiting time is (10 + 32 + 0 + 3 + 20)/5 = 13 milliseconds.

	With the RR algorithm, we execute the processes as	![[Pasted image 20260611155012.png]]

	5) The waiting time is 0 milliseconds for process _P_1, 32 milliseconds for process _P_2, 20 milliseconds for process _P_3, 23 milliseconds for process _P_4, and 40 milliseconds for process _P_5. Thus, the average waiting time is (0 + 32 + 20 + 23 + 40)/5 = 23 milliseconds.
	6) We see that, _in this case,_ the average waiting time obtained with the SJF policy is less than half that obtained with FCFS scheduling; the RR algorithm gives us an intermediate value.
3) Deterministic modeling is simple and fast. It gives us exact numbers, allowing us to compare the algorithms. However, it requires exact numbers for input, and its answers apply only to those cases. The main uses of deterministic modeling are in describing scheduling algorithms and providing examples.
4) In cases where we are running the same program over and over again and can measure the program’s processing requirements exactly, we may be able to use deterministic modeling to select a scheduling algorithm. 
5) Furthermore, over a set of examples, deterministic modeling may indicate trends that can then be analyzed and proved separately. For example, it can be shown that, for the environment described (all processes and their times available at time 0), the SJF policy will always result in the minimum waiting time.

## Related

- [[> Algorithm Evaluation]] — back to the sub-topic MOC
- [[Queueing Models]] — the next evaluation method
- [[Shortest-Job-First Scheduling]] — provably optimal in the modeled environment
- [[Selecting an Algorithm]] — defining the criteria first
