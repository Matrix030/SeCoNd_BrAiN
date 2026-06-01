---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.9.2"]
---

# Performance Tuning

1) To identify bottlenecks, we must be able to monitor system performance. Code must be added to compute and display measures of system behavior. 
2) In a number of systems, the operating system does this task by producing trace listings of system behavior. All interesting events are logged with their time and important parameters and are written to a file. 
3) Later, an analysis program can process the log file to determine system performance and to identify bottlenecks and inefficiencies. 
4) These same traces can be run as input for a simulation of a suggested improved system. Traces also can help people to find errors in operating-system behavior.

> [!note] Kernighan’s Law
> “Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.”

2) Another approach to performance tuning is to include interactive tools with the system that allow users and administrators to question the state of various components of the system to look for bottlenecks. 
3) The UNIX command top displays resources used on the system, as well as a sorted list of the “top” resource-using processes. 
4) Other tools display the state of disk I/O, memory allocation, and network traffic. The authors of these single-purpose tools try to guess what a user would want to see while analyzing a system and to provide that information.
5) Making running operating systems easier to understand, debug, and tune is an active area of operating system research and implementation. 
6) The cycle of enabling tracing as system problems occur and analyzing the traces later is being broken by a new generation of kernel-enabled performance analysis tools. 
7) Further, these tools are not single-purpose or merely for sections of code that were written to emit debugging data. The Solaris 10 DTrace dynamic tracing facility is a leading example of such a tool.

## Related

- [[> Operating-System Debugging]] — back to the sub-topic MOC
- [[Failure Analysis]] — the other side of debugging
- [[DTrace]] — the dynamic tracing facility introduced here
