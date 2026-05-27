---
tags: [book, os, operating-systems, book-os-concepts, security]
---

# Protection

1) If a computer system has multiple users and allows the concurrent execution of multiple processes, then access to data must be regulated.
2) For that purpose, mechanisms ensure that files, memory segments, CPU, and other resources can be operated on by only those processes that have gained proper authorization from the operating system.
3) For example:
	- Memory-addressing hardware ensures that a process can execute only within its own address space.
	- The timer ensures that no process can gain control of the CPU without eventually relinquishing control.
	- Device-control registers are not accessible to users, so the integrity of the various peripheral devices is protected.
4) **Protection**, then, is any mechanism for controlling the access of processes or users to the resources defined by a computer system. This mechanism must provide means to specify the controls to be imposed and means to enforce the controls.
5) Protection can improve reliability by detecting latent errors at the interfaces between component subsystems.
6) Early detection of interface errors can often prevent contamination of a healthy subsystem by another subsystem that is malfunctioning. Furthermore, an unprotected resource cannot defend against use (or misuse) by an unauthorized or incompetent user.
7) A protection-oriented system provides a means to distinguish between authorized and unauthorized usage, as we discuss in Chapter 14.

## Related

- [[> Protection and Security]] — back to the sub-topic MOC
- [[Security]] — security extends protection to external and internal attacks
- [[User and Group Identifiers]] — how the OS identifies users for protection
