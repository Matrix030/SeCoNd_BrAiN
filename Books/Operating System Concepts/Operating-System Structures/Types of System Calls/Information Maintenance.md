---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["2.4.4"]
---

# Information Maintenance

1) Many system calls exist simply for the purpose of transferring information between the user program and the operating system.
2) For example, most systems have a system call to return the current time and date. Other system calls may return information about the system, such as the number of current users, the version number of the operating system, the amount of free memory or disk space, and so on.
3) Another set of system calls is helpful in debugging a program. Many systems provide system calls to dump memory. This provision is useful for debugging. A program trace lists each system call as it is executed. Even microprocessors provide a CPU mode known as _single step,_ in which a trap is executed by the CPU after every instruction. The trap is usually caught by a debugger.
4) Many operating systems provide a time profile of a program to indicate the amount of time that the program executes at a particular location or set of locations. A time profile requires either a tracing facility or regular timer interrupts. At every occurrence of the timer interrupt, the value of the program counter is recorded.
5) In addition, the operating system keeps information about all its [[process|processes]], and system calls are used to access this information.

## Related

- [[> Types of System Calls]] — back to the sub-topic MOC
- [[Device Management]] — the previous category of system calls
- [[Communication]] — the next category of system calls
- [[process]] — the entities the OS tracks information about
