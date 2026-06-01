---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.8.5"]
---

# Implementation

1) Although the virtual-machine concept is useful, it is difficult to implement. Much work is required to provide an _exact_ duplicate of the underlying machine. 
2) Remember that the underlying machine typically has two modes: user mode and kernel mode. The virtual-machine software can run in kernel mode, since it is the operating system. The virtual machine itself can execute in only user mode. 
3) Just as the physical machine has two modes, however, so must the virtual machine. Consequently, we must have a virtual user mode and a virtual kernel mode, both of which run in a physical user mode. 
4) Those actions that cause a transfer from user mode to kernel mode on a real machine (such as a system call or an attempt to execute a privileged instruction) must also cause a transfer from virtual user mode to virtual kernel mode on a virtual machine.
5) Such a transfer can be accomplished as follows. 
	- When a system call, for example, is made by a program running on a virtual machine in virtual user mode, it will cause a transfer to the virtual-machine monitor in the real machine. 
	- When the virtual-machine monitor gains control, it can change the register contents and program counter for the virtual machine to simulate the effect of the system call. It can then restart the virtual machine, noting that it is now in virtual kernel mode.
	- The major difference, of course, is time. Whereas the real I/O might have taken 100 milliseconds, the virtual I/O might take less time (because it is spooled) or more time (because it is interpreted).
	- In addition, the CPU is being multiprogrammed among many virtual machines, further slowing down the virtual machines in unpredictable ways In the extreme case, it may be necessary to simulate all instructions to provide a true virtual machine. 
	- VM, discussed earlier, works for IBM machines because normal instructions for the virtual machines can execute directly on the hardware. Only the privileged instructions (needed mainly for I/O) must be simulated and hence execute more slowly.
6) Without some level of hardware support, virtualization would be impossible. The more hardware support available within a system, the more feature rich, stable, and well performing the virtual machines can be. 
7) All major general-purpose CPUs provide some amount of hardware support for virtualization. For example, AMD virtualization technology is found in several AMD processors. 
8) It defines two new modes of operation—host and guest. Virtual machine software can enable host mode, define the characteristics of each guest virtual machine, and then switch the system to guest mode, passing control of the system to the guest operating system that is running in the virtual machine. 
9) In guest mode, the virtualized operating system thinks it is running on native hardware and sees certain devices (those included in the host’s definition of the guest). If the guest tries to access a virtualized resource, then control is passed to the host to manage that interaction.

## Related

- [[> Virtual Machines]] — back to the sub-topic MOC
- [[Virtual Machines]] — the section intro
- [[Examples]] — contemporary virtual machines in practice
