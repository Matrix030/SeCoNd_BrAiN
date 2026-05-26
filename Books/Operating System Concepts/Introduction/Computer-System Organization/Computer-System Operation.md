---
tags: [book, os, operating-systems, book-os-concepts, io]
aliases: ["Computer System Operation", "1.2.1"]
---

# **1.2.1 Computer-System Operation**

1) A modern general-purpose computer system consists of one or more CPUs and a number of device controllers connected through a common bus that provides access to shared memory. 
2) Each device controller is in charge of a specific type of device (for example, disk drives, audio devices, and video displays).
3) The CPU and the device controllers can execute concurrently, competing for memory cycles. 
4) To **ensure orderly access to the shared memory**, a **memory controller** is provided whose **function is to synchronize access to the memory**.
		[TODO - wtf, how everything does from powering to final OS boot]
	1) For a computer to start running—for instance, when it is powered up or rebooted—it needs to have an initial program to run.
	2) This initial program, or **[[bootstrap program]]**, tends to be simple. 
	3) Typically, it is stored in read-only memory (**ROM**) or electrically erasable programmable read-only memory (**EEPROM**), known by the general term **[[firmware]]**, within the computer hardware. 
	4) It initializes all aspects of the system, from CPU registers to device controllers to memory contents. 
	5) The [[bootstrap program]] must know how to load the operating system and how to start executing that system. 
	6) To accomplish this goal, the [[bootstrap program]] must locate and load into memory the operating-system kernel.
	7) The operating system then starts executing the first process, such as "init," and waits for some event to occur.
	
![[Pasted image 20260525162750.png]]

5) The occurrence of an event is usually signaled by an **[[interrupt]]** from either the hardware or the software.
6) Hardware may trigger an [[interrupt]] at any time by sending a signal to the CPU, usually by way of the system bus. 
7) Software may trigger an [[interrupt]] by executing a special operation called a **[[system call]]** (also called a **monitor call**).
8) When the CPU is interrupted, it stops what it is doing and immediately transfers execution to a fixed location. The fixed location usually contains the starting address where the service routine for the [[interrupt]] is located.
9) The interrupt service routine executes; on completion, the CPU resumes the interrupted computation. A time line of this operation is shown in the figure below.
10) **Interrupts** are an important part of a computer architecture. 
11) Each computer design has its own interrupt mechanism, but several functions are common. 
12) The [[interrupt]] must transfer control to the appropriate interrupt service routine.
13) The straightforward method for handling this transfer would be to invoke a generic routine to examine the interrupt information; the routine, in turn, would call the interrupt-specific handler.
14) However, interrupts must be handled quickly. Since only a predefined number of interrupts is possible, a table of pointers to interrupt routines can be used instead to provide the necessary speed. 
15) The interrupt routine is called indirectly through the table, with no intermediate routine needed. 
16) Generally, the table of pointers is stored in low memory (the first hundred or so locations).
17) These locations hold the addresses of the interrupt service routines for the various devices. 
18) This array, or **[[interrupt vector]]**, of addresses is then indexed by a unique device number, given with the interrupt request, to provide the address of the interrupt service routine for the interrupting device. 
19) Operating systems as different as Windows and UNIX dispatch interrupts in this manner.

![[Pasted image 20260525162945.png]]

16) The interrupt architecture must also save the address of the interrupted instruction. 
17) Many old designs simply stored the interrupt address in a fixed location or in a location indexed by the device number. 
18) More recent architectures store the return address on the system stack. 
19) If the interrupt routine needs to modify the processor state—for instance, by modifying register values—it must explicitly save the current state and then restore that state before returning. 
20) After the [[interrupt]] is serviced, the saved return address is loaded into the program counter, and the interrupted computation resumes as though the interrupt had not occurred.

## Related

- [[> Computer-System Organization]] — back to the sub-topic MOC
- [[Storage Structure]] — memory types and hierarchy
- [[IO Structure]] — device controllers and DMA
