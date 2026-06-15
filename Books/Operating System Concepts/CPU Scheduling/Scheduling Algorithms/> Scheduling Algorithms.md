---
tags: [book, os, operating-systems, book-os-concepts, moc]
aliases: ["Scheduling Algorithms"]
---

# Scheduling Algorithms — Map of Content

> [!info] Section 5.3
> CPU scheduling deals with the problem of deciding which of the processes in the ready queue is to be allocated the CPU. There are many different CPU-scheduling algorithms. In this section, we describe several of them.

---

## Notes

- [[First-Come, First-Served Scheduling]] — the simplest algorithm; FIFO queue, nonpreemptive, and the convoy effect
- [[Shortest-Job-First Scheduling]] — provably optimal average wait; predicting the next burst via exponential average
- [[Priority Scheduling]] — highest-priority process first; starvation and aging
- [[Round-Robin Scheduling]] — time quantum and preemption for time-sharing systems
- [[Multilevel Queue Scheduling]] — partitioning the ready queue into separate, permanently-assigned queues
- [[Multilevel Feedback Queue Scheduling]] — the most general algorithm; processes move between queues

---

## Related

- [[> CPU Scheduling]] — back to the chapter MOC
