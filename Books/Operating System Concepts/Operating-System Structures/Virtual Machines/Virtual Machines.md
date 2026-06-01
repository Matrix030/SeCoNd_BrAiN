---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.8"]
---

# Virtual Machines

1) The [[Layered Approach|layered approach]] described in Section 2.7.2 is taken to its logical conclusion in the concept of a **virtual machine**. 
2) The fundamental idea behind a virtual machine is to abstract the hardware of a single computer into several different execution environments, thereby creating the illusion that each separate execution environment is running its own private computer.
3) By using CPU scheduling (Chapter 5) and virtual-memory techniques (Chapter 9), an operating system **host** can create the illusion that a process has its own processor with its own (virtual) memory. 
4) The virtual machine provides an interface that is _identical_ to the underlying bare hardware. Each **guest** process is provided with a (virtual) copy of the underlying computer (refer the figure below). Usually, the guest process is in fact an operating system, and that is how a single physical machine can run multiple operating systems concurrently, each in its own virtual machine.
![[Pasted image 20260601132222.png]]
5) A major difficulty with the VM virtual-machine approach involved disk systems. Suppose that the physical machine had three disk drives but wanted to support seven virtual machines.
6) Clearly, it could not allocate a disk drive to each virtual machine, because the virtual-machine software itself needed substantial disk space to provide virtual memory and spooling. 
7) The solution was to provide virtual disks—termed _minidisks_ in IBM’s VM operating system—that are identical in all respects except size. The system implemented each minidisk by allocating as many tracks on the physical disks as the minidisk needed.
	1) Disk 1  
		+-------------------------------------+  
		| VM1 | VM1 | VM1 | VM2 | VM2 | VM3 |  
		+-------------------------------------+  
		
		Disk 2  
		+-------------------------------------+  
		| VM4 | VM4 | VM5 | VM5 | VM6 | VM7 |  
		+-------------------------------------+
1) Once these virtual machines were created, users could run any of the operating systems or software packages that were available on the underlying machine.

## Related

- [[> Virtual Machines]] — back to the sub-topic MOC
- [[Benefits]] — why virtual machines are created
- [[Implementation]] — how virtual machines are realized
- [[Layered Approach]] — the structuring idea taken to its conclusion
