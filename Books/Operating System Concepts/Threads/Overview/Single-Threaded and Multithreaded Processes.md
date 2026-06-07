---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.1"]
---

# Single-Threaded and Multithreaded Processes

1) A thread is a basic unit of CPU utilization; it comprises a thread ID, a program counter, a register set, and a stack. It shares with other threads belonging to the same process its code section, data section, and other operating-system resources, such as open files and signals. 
2) A traditional (or **heavyweight**) process has a single thread of control. If a process has multiple threads of control, it can perform more than one task at a time. The figure below illustrates the difference between a traditional **single-threaded** process and a **multithreaded** process.

## Related

- [[> Overview]] — back to the sub-topic MOC
- [[Motivation]] — why applications use multiple threads
- [[The Process]] — the heavyweight process this builds on
- [[Threads]] — threads within the process concept
