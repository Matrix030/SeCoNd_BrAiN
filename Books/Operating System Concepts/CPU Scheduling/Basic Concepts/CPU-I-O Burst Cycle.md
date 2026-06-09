---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["CPU-I/O Burst Cycle", "CPU Burst", "I/O Burst"]
---

# CPU-I/O Burst Cycle

1) The success of CPU scheduling depends on an observed property of processes: process execution consists of a **cycle** of CPU execution and I/O wait. Processes alternate between these two states. Process execution begins with a **CPU burst**. That is followed by an **I/O burst**, which is followed by another CPU burst, then another I/O burst, and so on. Eventually, the final CPU burst ends with a system request to terminate execution (see the figure below).

The durations of CPU bursts have been measured extensively. Although they vary greatly from process to process and from computer to computer, they tend to have a frequency curve similar to that shown in the figure below. The curve is generally characterized as exponential or hyperexponential, with a large number of short CPU bursts and a small number of long CPU bursts. An I/O-bound program typically has many short CPU bursts. A CPU-bound program might have a few long CPU bursts. This distribution can be important in the selection of an appropriate CPU-scheduling algorithm.

## Related

- [[> Basic Concepts]] — back to the sub-topic MOC
- [[Multiprogramming]] — why this cycle matters for CPU utilization
- [[CPU Scheduler]] — uses this property to select processes
