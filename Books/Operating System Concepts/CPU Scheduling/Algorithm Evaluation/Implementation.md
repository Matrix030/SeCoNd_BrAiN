---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Implementation"]
---

# Implementation

1) Even a simulation is of limited accuracy. The only completely accurate way to evaluate a scheduling algorithm is to code it up, put it in the operating system, and see how it works. 
2) This approach puts the actual algorithm in the real system for evaluation under real operating conditions.
3) The major difficulty with this approach is the high cost. The expense is incurred not only in coding the algorithm and modifying the operating system to support it (along with its required data structures) but also in the reaction of the users to a constantly changing operating system.
4) Another difficulty is that the environment in which the algorithm is used will change. The environment will change not only in the usual way, as new programs are written and the types of problems change, but also as a result of the performance of the scheduler. 
5) If short processes are given priority, then users may break larger processes into sets of smaller processes. If interactive processes are given priority over noninteractive processes, then users may switch to interactive use.
6) For example, researchers designed one system that classified interactive and noninteractive processes automatically by looking at the amount of terminal I/O. 
7) If a process did not input or output to the terminal in a 1-second interval, the process was classified as noninteractive and was moved to a lower-priority queue. In response to this policy, one programmer modified his programs to write an arbitrary character to the terminal at regular intervals of less than 1 second. 
8) The system gave his programs a high priority, even though the terminal output was completely meaningless.
9) The most flexible scheduling algorithms are those that can be altered by the system managers or by the users so that they can be tuned for a specific application or set of applications. 
10) A workstation that performs high-end graphical applications, for instance, may have scheduling needs different from those of a Web server or file server. Some operating systems—particularly several versions of UNIX—allow the system manager to fine-tune the scheduling parameters for a particular system configuration.
11) Another approach is to use APIs that modify the priority of a process or thread. The Java, /POSIX, and /WinAPI/ provide such functions.
12) The downfall of this approach is that performance-tuning a system or application most often does not result in improved performance in more general situations.

## Related

- [[> Algorithm Evaluation]] — back to the sub-topic MOC
- [[Simulations]] — the less costly, less accurate alternative
- [[Priority Scheduling]] — the kind of policy users learn to game
