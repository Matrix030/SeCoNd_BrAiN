---
tags: [book, os, operating-systems, book-os-concepts, processes, scheduling]
aliases: ["3.2"]
---

# Scheduling Objectives

1) The objective of multiprogramming is to have some process running at all times, to maximize CPU utilization. 
2) The objective of time sharing is to switch the CPU among processes so frequently that users can interact with each program while it is running. 
3) To meet these objectives, the **process scheduler** selects an available process (possibly from a set of several available processes) for program execution on the CPU. 
4) For a single-processor system, there will never be more than one running process. If there are more processes, the rest will have to wait until the CPU is free and can be rescheduled.

## Related

- [[> Process Scheduling]] — back to the sub-topic MOC
- [[Scheduling Queues]] — the queues a process moves through
- [[Schedulers]] — the schedulers that select processes
