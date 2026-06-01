---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.8.2"]
---

# Benefits

1) There are several reasons for creating a virtual machine. Most of them are fundamentally related to being able to share the same hardware yet run several different execution environments (that is, different operating systems) concurrently.
2) One important advantage is that the host system is protected from the virtual machines, just as the virtual machines are protected from each other. A virus inside a guest operating system might damage that operating system but is unlikely to affect the host or the other guests. Because each virtual machine is completely isolated from all other virtual machines, there are no protection problems.
3) At the same time, however, there is no direct sharing of resources. Two approaches to provide sharing have been implemented. 
	1) First, it is possible to share a file-system volume and thus to share files. 
	2) Second, it is possible to define a network of virtual machines, each of which can send information over the virtual communications network. 
4) The network is modeled after physical communication networks but is implemented in software.
5) A virtual-machine system is a perfect vehicle for operating-systems research and development. Normally, changing an operating system is a difficult task. Operating systems are large and complex programs, and it is difficult to be sure that a change in one part will not cause obscure bugs to appear in some other part.
6) The OS runs in **kernel mode**, meaning it has full access to memory, disks, and hardware. A small bug (such as a bad pointer) can corrupt critical data or even destroy the filesystem, so OS changes must be tested carefully.
7) Since the OS controls the entire machine, you cannot easily modify and test it while users are actively using it. The current system usually has to be stopped before testing new OS changes.
8) The time reserved for making and testing OS changes is called **system-development time**. Because the system is unavailable during this period, it is typically scheduled at night or on weekends when fewer users are affected.
9) A virtual-machine system can eliminate much of this problem. System programmers are given their own virtual machine, and system development is done on the virtual machine instead of on a physical machine. Normal system operation seldom needs to be disrupted for system development.
10) Another advantage of virtual machines for developers is that multiple operating systems can be running on the developer’s workstation concurrently. This virtualized workstation allows for rapid porting and testing of programs in varying environments.
11) A major advantage of virtual machines in production data-center use is system **consolidation**, which involves taking two or more separate systems and running them in virtual machines on one system. Such physical-to-virtual conversions result in resource optimization, as many lightly used systems can be combined to create one more heavily used system.
12) Instead of installing applications directly on a computer, developers can package the application together with a pre-configured operating system inside a **virtual machine (VM)**.
	Benefits:
	- **Developers:** Easier deployment, less configuration, and simpler support.
	- **System Administrators:** Easier installation, management, and migration between systems.
	- **Users:** Applications work in a known, pre-tested environment.
	For this approach to work everywhere, VM formats need to be standardized so that a VM created on one platform can run on another. **Open Virtual Machine Format (OVF)** was created to help achieve this compatibility.

## Related

- [[> Virtual Machines]] — back to the sub-topic MOC
- [[Virtual Machines]] — the section intro
- [[Implementation]] — how these benefits are realized
