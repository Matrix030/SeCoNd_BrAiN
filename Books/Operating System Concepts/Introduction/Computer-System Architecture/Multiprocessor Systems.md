---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["1.3.2", "parallel systems", "tightly coupled systems"]
---

# **1.3.2 Multiprocessor Systems**

1) Although single-processor systems are most common, **multiprocessor systems** (also known as **parallel systems** or **tightly coupled systems**) are growing in importance. Such systems have two or more processors in close communication, sharing the computer bus and sometimes the clock, memory, and peripheral devices.
Multiprocessor systems have three main advantages:
	1. **Increased throughput**. By increasing the number of processors, we expect to get more work done in less time. The speed-up ratio with _N_ processors is not _N,_ however; **rather, it is less than _N**._ When multiple processors cooperate on a task, a certain amount of overhead is incurred in keeping all the parts working correctly. This overhead, plus contention for shared resources, lowers the expected gain from additional processors. Similarly, _N_ programmers working closely together do not produce _N_ times the amount of work a single programmer would produce.
	2. **Economy of scale**. Multiprocessor systems can cost less than equivalent multiple single-processor systems, because they can share peripherals, mass storage, and power supplies. If several programs operate on the same set of data, it is cheaper to store those data on one disk and to have all the processors share them than to have many computers with local disks and many copies of the data.
	3. **Increased reliability**. If functions can be distributed properly among several processors, then the failure of one processor will not halt the system, only slow it down. If we have ten processors and one fails, then each of the remaining nine processors can pick up a share of the work of the failed processor. Thus, the entire system runs only 10 percent slower, rather than failing altogether.

2) Increased reliability of a computer system is crucial in many applications. The ability to continue providing service proportional to the level of surviving hardware is called **[[graceful degradation]]**. 
3) Some systems go beyond graceful degradation and are called **fault tolerant**, because they can suffer a failure of any single component and still continue operation. Note that fault tolerance requires a mechanism to allow the failure to be detected, diagnosed, and, if possible, corrected. 
4) The multiple-processor systems in use today are of two types. Some systems use **asymmetric multiprocessing**, in which each processor is assigned a specific task. A master processor controls the system; the other processors either look to the master for instruction or have predefined tasks. This scheme defines a master-slave relationship. The master processor schedules and allocates work to the slave processors.
5) The most common systems use **symmetric multiprocessing ([[SMP]])**, in which each processor performs all tasks within the operating system. SMP means that all processors are peers; no master-slave relationship exists between processors. 
6) The figure illustrates a typical [[SMP]] architecture. Notice that each processor has its own set of registers, as well as a private—or local—cache; however, all processors share physical memory. 
7) However, we must carefully control I/O to ensure that the data reach the appropriate processor. Also, since the CPUs are separate, one may be sitting idle while another is overloaded, resulting in inefficiencies. 
8) These inefficiencies can be avoided if the processors **share certain data structures**. 
9) A multiprocessor system of this form will allow processes and resources—such as memory—to be shared dynamically among the various processors and can lower the variance among the processors.

![[Pasted image 20260526143345.png]]

10) The difference between symmetric and asymmetric multiprocessing may result from either hardware or software. Special hardware can differentiate the multiple processors, or the software can be written to allow only one master and multiple slaves. 
11) Multiprocessing adds CPUs to increase computing power. If the CPU has an integrated memory controller, then adding CPUs can also increase the amount of memory addressable in the system. 
12) Multiprocessing can cause a system to change its memory access model from uniform memory access (**[[UMA]]**) to non-uniform memory access (**[[NUMA]]**). 
	1) UMA is defined as the situation in which access to any RAM from any CPU takes the same amount of time. 
	2) With [[NUMA]], some parts of memory may take longer to access than other parts, creating a performance penalty. Operating systems can minimize the NUMA penalty through resource management, as discussed in Section 9.5.4.
13) A recent trend in CPU design is to include multiple computing **cores** on a single chip. In essence, these are multiprocessor chips. 
14) They can be more efficient than multiple chips with single cores because on-chip communication is faster than between-chip communication. In addition, one chip with multiple cores uses significantly less power than multiple single-core chips. As a result, multicore systems are especially well suited for server systems such as database and Web servers.
15) In the below figure show a dual-core design with two cores on the same chip. In this design, each core has its own register set as well as its own local cache; other designs might use a shared cache or a combination of local and shared caches. Aside from architectural considerations, such as cache, memory, and bus contention, these multicore CPUs appear to the operating system as _N_ standard processors. This tendency puts pressure on operating system designers—and application programmers—to make use of those CPUs.
![[Pasted image 20260526145048.png]]

16) Finally, **blade servers** are a recent development in which multiple processor boards, I/O boards, and networking boards are placed in the same chassis. The difference between these and traditional multiprocessor systems is that each blade-processor board boots independently and runs its own operating system. Some blade-server boards are multiprocessor as well, which blurs the lines between types of computers. In essence, these servers consist of multiple independent multiprocessor systems.

## Related

- [[> Computer-System Architecture]] — back to the sub-topic MOC
- [[Single-Processor Systems]] — single general-purpose CPU systems
- [[Clustered Systems]] — multiple independent systems joined together
