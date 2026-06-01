---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.7.4"]
---

# Modules

1) Perhaps the best current methodology for operating-system design involves using object-oriented programming techniques to create a modular kernel.
2) Here, the kernel has a set of core components and links in additional services either during boot time or during run time. Such a strategy uses dynamically loadable modules and is common in modern implementations of UNIX. For example, the Solaris operating system structure, shown in the below figure is organized around a core kernel with seven types of loadable kernel modules:
	1. Scheduling classes
	2. File systems
	3. Loadable system calls
	4. Executable formats
	5. STREAMS modules
	6. Miscellaneous
	7. Device and bus drivers
3) Such a design allows the kernel to provide core services yet also allows certain features to be implemented dynamically. For example, device and bus drivers for specific hardware can be added to the kernel, and support for different file systems can be added as loadable modules.
4) The overall result resembles a layered system in that each kernel section has defined, protected interfaces; but it is more flexible than a layered system in that any module can call any other module.
5) Furthermore, the approach is like the microkernel approach in that the primary module has only core functions and knowledge of how to load and communicate with other modules; but it is more efficient, because modules do not need to invoke message passing in order to communicate.

![[Pasted image 20260529212358.png]]

6) The Apple Mac OS X operating system uses a hybrid structure. It is a layered system in which one layer consists of the Mach microkernel. The structure of Mac OS X appears in the figure below, The top layers include application environments and a set of services providing a graphical interface to applications. Below these layers is the kernel environment, which consists primarily of the Mach microkernel and the BSD kernel. Mach provides memory management; support for remote procedure calls (RPCs) and interprocess communication (IPC) facilities, including message passing; and thread scheduling.

![[Pasted image 20260529212409.png]]

7) In addition to Mach and BSD, the kernel environment provides an I/O kit for development of device drivers and dynamically loadable modules (which Mac OS X refers to as **kernel extensions**). As shown in the figure, applications and common services can make use of either the Mach or BSD facilities directly.

## Related

- [[> Operating-System Structure]] — back to the sub-topic MOC
- [[Layered Approach]] — which a modular kernel resembles
- [[Microkernels]] — which a modular kernel borrows from
