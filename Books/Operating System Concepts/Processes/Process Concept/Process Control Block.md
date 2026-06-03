---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.1.3", "PCB"]
---

# Process Control Block

1) Each process is represented in the operating system by a **process control block** (**PCB**)—also called a _task control block_. 
2) A PCB is shown in the figure below. 

![[Pasted image 20260601204528.png]]
3) It contains many pieces of information associated with a specific process, including these:
	- **Process state**. The state may be new, ready, running, waiting, halted, and so on.
	- **Program counter**. The counter indicates the address of the next instruction to be executed for this process.
	- **CPU registers**. The registers vary in number and type, depending on the computer architecture. They include accumulators, index registers, stack pointers, and general-purpose registers, plus any condition-code information. Along with the program counter, this state information must be saved when an interrupt occurs, to allow the process to be continued correctly afterward (see figure below).
	- **CPU-scheduling information**. This information includes a process priority, pointers to scheduling queues, and any other scheduling parameters.
	- **Memory-management information**. This information may include such information as the value of the base and limit registers, the page tables, or the segment tables, depending on the memory system used by the operating system.
	- **Accounting information**. This information includes the amount of CPU and real time used, time limits, account numbers, job or process numbers, and so on.
	- **I/O** status information. This information includes the list of I/O devices allocated to the process, a list of open files, and so on.

## Related

- [[> Process Concept]] — back to the sub-topic MOC
- [[Process State]] — one of the fields stored in the PCB
- [[The Process]] — what the PCB represents
- [[Threads]] — how the PCB is expanded for multithreaded processes
