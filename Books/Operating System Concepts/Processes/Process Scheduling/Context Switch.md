---
tags: [book, os, operating-systems, book-os-concepts, processes, scheduling]
aliases: ["3.2.3"]
---

# Context Switch

1) As mentioned in Section 1.2.1, interrupts cause the operating system to change a CPU from its current task and to run a kernel routine. 
2) Such operations happen frequently on general-purpose systems. When an interrupt occurs, the system needs to save the current **context** of the process running on the CPU so that it can restore that context when its processing is done, essentially suspending the process and then resuming it. The context is represented in the [[Process Control Block|PCB]] of the process; it includes the value of the CPU registers, the process state, and memory-management information. Generically, we perform a **state save** of the current state of the CPU, be it in kernel or user mode, and then a **state restore** to resume operations.
3) Switching the CPU to another process requires performing a state save of the current process and a state restore of a different process. This task is known as a **context switch**.
4) When a context switch occurs, the kernel saves the context of the old process in its PCB and loads the saved context of the new process scheduled to run. 
5) Context-switch time is pure overhead, because the system does no useful work while switching. Its speed varies from machine to machine, depending on the memory speed, the number of registers that must be copied, and the existence of special instructions (such as a single instruction to load or store all registers). Typical speeds are a few milliseconds.
6) Context-switch times are highly dependent on hardware support. For instance, some processors (such as the Sun UltraSPARC) provide multiple sets of registers. A context switch here simply requires changing the pointer to the current register set.
7) Of course, if there are more active processes than there are register sets, the system resorts to copying register data to and from memory, as before.
8) Also, the more complex the operating system, the more work must be done during a context switch. As we will see in Chapter 8, advanced memory-management techniques may require extra data to be switched with each context. 
9) For instance, the address space of the current process must be preserved as the space of the next task is prepared for use. 
10) How the address space is preserved, and what amount of work is needed to preserve it, depend on the memory-management method of the operating system.

## Related

- [[> Process Scheduling]] — back to the sub-topic MOC
- [[Process Control Block]] — where a process's context is saved and restored
- [[Schedulers]] — what decides which process to switch to
- [[Process State]] — the states a process moves through across a switch
