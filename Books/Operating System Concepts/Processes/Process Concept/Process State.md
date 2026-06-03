---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.1.2"]
---

# Process State

1) As a process executes, it changes **state**.
2) The state of a process is defined in part by the current activity of that process.
3) Each process may be in one of the following states:
	- **New**: The process is being created.
	- **Running**: Instructions are being executed.
	- Waiting: The process is waiting for some event to occur (such as an I/O completion or reception of a signal).
	- **Ready**: The process is waiting to be assigned to a processor.
	- **Terminated**: The process has finished execution.
4) These names are arbitrary, and they vary across operating systems. 
5) The states that they represent are found on all systems, however. 
6) Certain operating systems also more finely delineate process states. 
7) It is important to realize that only one process can be _running_ on any processor at any instant. Many processes may be _ready_ and _waiting,_ however. The state diagram corresponding to these states is presented in the figure below.

![[Pasted image 20260601204516.png]]

## Related

- [[> Process Concept]] — back to the sub-topic MOC
- [[The Process]] — what a process is made of
- [[Process Control Block]] — where the current state is recorded
- [[Process Scheduling]] — how the OS moves processes between states
