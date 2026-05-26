---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["1.3.1"]
---

# **1.3.1 Single-Processor Systems**

1) Most systems use a single processor. The variety of single-processor systems may be surprising, however, since these systems range from PDAs through mainframes.
2) On a single-processor system, there is one main CPU capable of executing a general-purpose instruction set, including instructions from user processes. Almost all systems have other special-purpose processors as well. 
3) They may come in the form of device-specific processors, such as disk, keyboard, and graphics controllers; or, on mainframes, they may come in the form of more general-purpose processors, such as I/O processors that move data rapidly among the components of the system.

![[Pasted image 20260526133651.png]]

4) All of these special-purpose processors run a limited instruction set and do not run user processes. 
5) Sometimes they are managed by the operating system, in that the operating system sends them information about their next task and monitors their status. For example, a disk-controller microprocessor receives a sequence of requests from the main CPU and implements its own disk queue and scheduling algorithm. 
6) This arrangement relieves the main CPU of the overhead of disk scheduling. PCs contain a microprocessor in the keyboard to convert the keystrokes into codes to be sent to the CPU.
7) In other systems or circumstances, special-purpose processors are low-level components built into the hardware. The operating system cannot communicate with these processors; they do their jobs autonomously. 
8) The use of special-purpose microprocessors is common and does not turn a single-processor system into a multiprocessor. If there is only one general-purpose CPU, then the system is a single-processor system.

## Related

- [[> Computer-System Architecture]] — back to the sub-topic MOC
- [[Multiprocessor Systems]] — the next step up from single-processor systems
- [[Computer-System Organization]] — how the hardware components connect
