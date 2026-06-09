---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["CPU Scheduling Introduction"]
---

# CPU Scheduling

CPU scheduling is the basis of multiprogrammed operating systems. By switching the CPU among processes, the operating system can make the computer more productive. In this chapter, we introduce basic CPU-scheduling concepts and present several CPU-scheduling algorithms. We also consider the problem of selecting an algorithm for a particular system.

In Chapter 4, we introduced threads to the process model. On operating systems that support them, it is kernel-level threads—not processes—that are in fact being scheduled by the operating system. However, the terms process scheduling and thread scheduling are often used interchangeably. In this chapter, we use process scheduling when discussing general scheduling concepts and thread scheduling to refer to thread-specific ideas.

## Chapter Objectives

- To introduce CPU scheduling, which is the basis for multiprogrammed operating systems.
- To describe various CPU-scheduling algorithms.
- To discuss evaluation criteria for selecting a CPU-scheduling algorithm for a particular system.

## Related

- [[> CPU Scheduling]] — back to the chapter MOC
- [[> Threads]] — the previous chapter; kernel-level threads are what get scheduled
- [[> Process Scheduling]] — the scheduling queues and schedulers introduced earlier
