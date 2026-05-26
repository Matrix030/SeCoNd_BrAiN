---
tags: [book, os, operating-systems, book-os-concepts]
aliases: []
---

1) As mentioned earlier, modern operating systems are **interrupt driven**. If there are no processes to execute, no I/O devices to service, and no users to whom to respond, an operating system will sit quietly, waiting for something to happen. 
2) Events are almost always signaled by the occurrence of an [[interrupt]] or a trap. A **[[trap]]** (or an **exception**) is a software-generated [[interrupt]] caused either by an error (for example, division by zero or invalid memory access) or by a specific request from a user program that an operating-system service be performed. 
3) The interrupt-driven nature of an operating system defines that system's general structure. For each type of [[interrupt]], separate segments of code in the operating system determine what action should be taken. An interrupt service routine is provided that is responsible for dealing with the [[interrupt]].
4) Since the operating system and the users share the hardware and software resources of the computer system, we need to make sure that an error in a user program could cause problems only for the one program running. With sharing, many processes could be adversely affected by a bug in one program. For example, if a process gets stuck in an infinite loop, this loop could prevent the correct operation of many other processes. 
5) More subtle errors can occur in a [[Multiprogramming|multiprogramming]] system, where one erroneous program might modify another program, the data of another program, or even the operating system itself.
6) Without protection against these sorts of errors, either the computer must execute only one process at a time or all output must be suspect. 
7) A properly designed operating system must ensure that an incorrect (or malicious) program cannot cause other programs to execute incorrectly.

## Related

- [[> Operating-System Operations]] — back to the sub-topic MOC
- [[Dual-Mode Operation]] — how hardware enforces the boundary between OS and user code
- [[Timer]] — preventing user programs from monopolizing the CPU
