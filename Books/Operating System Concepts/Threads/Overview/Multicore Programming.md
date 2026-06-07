---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.1.3"]
---

# Multicore Programming

1) A recent trend in system design has been to place multiple computing cores on a single chip, where each core appears as a separate processor to the operating system (Section 1.3.2).
2) Multithreaded programming provides a mechanism for more efficient use of multiple cores and improved concurrency. 
3) Consider an application with four threads. On a system with a single computing core, concurrency merely means that the execution of the threads will be interleaved over time (sedd the figure 4.3 below), as the processing core is capable of executing only one thread at a time. On a system with multiple cores, however, concurrency means that the threads can run in parallel, as the system can assign a separate thread to each core (see figure 4.4).

![[Pasted image 20260607095453.png]]

![[Pasted image 20260607095459.png]]

4) The trend towards multicore systems has placed pressure on system designers as well as application programmers to make better use of the multiple computing cores. 
5) Designers of operating systems must write scheduling algorithms that use multiple processing cores to allow the parallel execution shown in the figure above. For application programmers, the challenge is to modify existing programs as well as design new programs that are multithreaded to take advantage of multicore systems. 
6) In general, five areas present challenges in programming for multicore systems:
	1. **Dividing activities**. This involves examining applications to find areas that can be divided into separate, concurrent tasks and thus can run in parallel on individual cores.
	2. **Balance**. While identifying tasks that can run in parallel, programmers must also ensure that the tasks perform equal work of equal value. In some instances, a certain task may not contribute as much value to the overall process as other tasks; using a separate execution core to run that task may not be worth the cost.
	3. **Data splitting**. Just as applications are divided into separate tasks, the data accessed and manipulated by the tasks must be divided to run on separate cores.
	4. **Data dependency**. The data accessed by the tasks must be examined for dependencies between two or more tasks. In instances where one task depends on data from another, programmers must ensure that the execution of the tasks is synchronized to accommodate the data dependency.
	5. **Testing and debugging**. When a program is running in parallel on multiple cores, there are many different execution paths. Testing and debugging such concurrent programs is inherently more difficult than testing and debugging single-threaded applications.

## Related

- [[> Overview]] — back to the sub-topic MOC
- [[Benefits]] — scalability, the benefit this expands on
- [[Multiprocessor Systems]] — the hardware that makes parallelism possible
- [[Single-Processor Systems]] — the contrast where threads only interleave
