---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.8.6.1"]
---

# VMware

1) Most of the virtualization techniques discussed in this section require virtualization to be supported by the kernel. Another method involves writing the virtualization tool to run in user mode as an application on top of the operating system. Virtual machines running within this tool believe they are running on bare hardware but in fact are running inside a user-level application.
2) **VMware** Workstation is a popular commercial application that abstracts Intel X86 and compatible hardware into isolated virtual machines.
3) VMware Workstation runs as an application on a host operating system such as Windows or Linux and allows this host system to concurrently run several different guest operating systems as independent virtual machines.
4) The architecture of such a system is shown in the figure below. In this scenario, Linux is running as the host operating system; and FreeBSD, Windows NT, and Windows XP are running as guest operating systems. 
5) The virtualization layer is the heart of VMware, as it abstracts the physical hardware into isolated virtual machines running as guest operating systems.
6) Each virtual machine has its own virtual CPU, memory, disk drives, network interfaces, and so forth.

![[Pasted image 20260601132258.png]]

7) The physical disk the guest owns and manages is really just a file within the file system of the host operating system. To create an identical guest instance, we can simply copy the file. Copying the file to another location protects the guest instance against a disaster at the original site. Moving the file to another location moves the guest system. These scenarios show how virtualization can improve the efficiency of system administration as well as system resource use.

## Related

- [[> Examples]] — back to the Examples MOC
- [[The Java Virtual Machine]] — the other example
- [[Implementation]] — how virtual machines are implemented
