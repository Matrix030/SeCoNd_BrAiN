---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["1.5.1", "user mode", "kernel mode"]
---

# **1.5.1 Dual-Mode Operation**

1) In order to ensure the proper execution of the operating system, we must be able to distinguish between the execution of operating-system code and user-defined code. The approach taken by most computer systems is to provide hardware support that allows us to differentiate among various modes of execution.
2) At the very least, we need two separate **modes** of operation: **[[user mode]]** and **[[kernel mode]]** (also called **supervisor mode**, **system mode**, or **privileged mode**). A bit, called the **mode bit**, is added to the hardware of the computer to indicate the current mode:
	 - kernel (0)
	 - user (1). 
3) With the mode bit, we are able to distinguish between a task that is executed on behalf of the operating system and one that is executed on behalf of the user. When the computer system is executing on behalf of a user application, the system is in user mode. 
4) However, when a user application requests a service from the operating system (via a [[system call]]), it must transition from user to kernel mode to fulfill the request. As we shall see, this architectural enhancement is useful for many other aspects of system operation as well.
![[Pasted image 20260526180258.png]]
 5) At system boot time, the hardware starts in kernel mode. The operating system is then loaded and starts user applications in user mode. 
 6) Whenever a [[trap]] or [[interrupt]] occurs, the hardware switches from user mode to kernel mode (that is, changes the state of the mode bit to 0). Thus, whenever the operating system gains control of the computer, it is in kernel mode. The system always switches to user mode (by setting the mode bit to 1) before passing control to a user program.
 7) The dual mode of operation provides us with the means for protecting the operating system from errant users—and errant users from one another. We accomplish this protection by designating some of the machine instructions that may cause harm as **privileged instructions**. 
 8) The hardware allows privileged instructions to be executed only in kernel mode. If an attempt is made to execute a privileged instruction in user mode, the hardware does not execute the instruction but rather treats it as illegal and traps it to the operating system.
 9) The instruction to switch to kernel mode is an example of a privileged instruction. Some other examples include I/O control, timer management, and interrupt management. As we shall see throughout the text, there are many additional privileged instructions.
 10) We can now see the life cycle of instruction execution in a computer system. Initial control resides in the operating system, where instructions are executed in kernel mode. When control is given to a user application, the mode is set to user mode. Eventually, control is switched back to the operating system via an [[interrupt]], a [[trap]], or a [[system call]].
 11) [[System call|System calls]] provide the means for a user program to ask the operating system to perform tasks reserved for the operating system on the user program's behalf. In all forms, it is the method used by a process to request action by the operating system.
 12) A [[system call]] usually takes the form of a [[trap]] to a specific location in the [[interrupt vector]]. This trap can be executed by a generic trap instruction, although some systems (such as the MIPS R2000 family) have a specific syscall instruction.
 13) When a [[system call]] is executed, it is treated by the hardware as a software interrupt. Control passes through the [[interrupt vector]] to a service routine in the operating system, and the mode bit is set to kernel mode.
 14) The system-call service routine is a part of the operating system. The kernel examines the interrupting instruction to determine what system call has occurred; a parameter indicates what type of service the user program is requesting. 
 15) Additional information needed for the request may be passed in registers, on the stack, or in memory (with pointers to the memory locations passed in registers). 
 16) The kernel verifies that the parameters are correct and legal, executes the request, and returns control to the instruction following the [[system call]].
 17) If a user program fails in some way—such as by making an attempt either to execute an illegal instruction or to access memory that is not in the user's address space—then the hardware traps to the operating system. 
 18) The [[trap]] transfers control through the [[interrupt vector]] to the operating system, just as an [[interrupt]] does. When a program error occurs, the operating system must terminate the program abnormally. This situation is handled by the same code as a user-requested abnormal termination.
 19) An appropriate error message is given, and the memory of the program may be dumped. The memory dump is usually written to a file so that the user or programmer can examine it and perhaps correct it and restart the program.

## Related

- [[> Operating-System Operations]] — back to the sub-topic MOC
- [[Timer]] — another mechanism for OS to retain control
- [[Computer-System Operation]] — interrupts and how the CPU handles them at the hardware level
