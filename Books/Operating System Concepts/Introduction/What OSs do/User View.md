---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["User View"]
---

# User View

1) The user’s view of the computer varies according to the interface being used.
2) Most computer users sit in front of a PC, consisting of a monitor, keyboard, mouse, and system unit. Such a system is designed for one user to monopolize its resources. The goal is to maximize the work (or play) that the user is performing. 
	1) In this case, the operating system is designed mostly for **ease of use**, with some attention paid to performance and none paid to resource **utilization**—how various hardware and software resources are shared. 
	2) In other cases, a user sits at a terminal connected to a **mainframe** or a **minicomputer**. 
		- Other users are accessing the same computer through other terminals. These users share resources and may exchange information. 
		- The operating system in such cases is designed to maximize resource utilization—to assure that all available CPU time, memory, and I/O are used efficiently and that no individual user takes more than her fair share.
	3) In still other cases, users sit at **workstations** connected to networks of other workstations and **servers**. 
		- These users have dedicated resources at their disposal, but they also share resources such as networking and servers—file, compute, and print servers. 
		- Therefore, their operating system is designed to compromise between individual usability and resource utilization.

3) Recently, many varieties of handheld computers have come into fashion.
4) Most of these devices are standalone units for individual users. 
5) Some are connected to networks, either directly by wire or (more often) through wireless modems and networking.
6) Because of power, speed, and interface limitations, they perform relatively few remote operations. 
7) Their operating systems are designed mostly for individual usability, but performance per unit of battery life is important as well.
8) Some computers have little or no user view. For example, embedded computers in home devices and automobiles may have numeric keypads and may turn indicator lights on or off to show status, but they and their operating systems are designed primarily to run without user intervention.

## Related

- [[> What OSs do]] — back to the sub-topic MOC
- [[System View]] — the same OS seen from the system's side
- [[Computer System Components]] — where the user sits among the four components
