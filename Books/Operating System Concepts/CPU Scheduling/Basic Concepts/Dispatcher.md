---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Dispatcher", "Dispatch Latency"]
---

# Dispatcher

1) Another component involved in the CPU-scheduling function is the **dispatcher**. The dispatcher is the module that gives control of the CPU to the process selected by the short-term scheduler. This function involves the following:
	- Switching context
	- Switching to user mode
	- Jumping to the proper location in the user program to restart that program
2) The dispatcher should be as fast as possible, since it is invoked during every process switch. The time it takes for the dispatcher to stop one process and start another running is known as the **dispatch latency**.

## Related

- [[> Basic Concepts]] — back to the sub-topic MOC
- [[CPU Scheduler]] — selects the process the dispatcher hands the CPU to
- [[Preemptive Scheduling]] — the decisions that trigger a dispatch
