---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Multiprogramming and CPU Scheduling"]
---

# Multiprogramming

1) In a single-processor system, only one process can run at a time; any others must wait until the CPU is free and can be rescheduled. The objective of [[Multiprogramming|multiprogramming]] is to have some process running at all times, to maximize CPU utilization. 
2) The idea is relatively simple. A process is executed until it must wait, typically for the completion of some I/O request. In a simple computer system, the CPU then just sits idle. All this waiting time is wasted; no useful work is accomplished. 
3) With multiprogramming, we try to use this time productively. Several processes are kept in memory at one time. When one process has to wait, the operating system takes the CPU away from that process and gives the CPU to another process. This pattern continues. 
4) Every time one process has to wait, another process can take over use of the CPU.

see the figure above

5) Scheduling of this kind is a fundamental operating-system function. Almost all computer resources are scheduled before use. 
6) The CPU is, of course, one of the primary computer resources. Thus, its scheduling is central to operating-system design.

## Related

- [[> Basic Concepts]] — back to the sub-topic MOC
- [[CPU-I/O Burst Cycle]] — the property of processes that CPU scheduling exploits
- [[CPU Scheduler]] — the component that selects which process runs
- [[Processes]] — the unit being scheduled
