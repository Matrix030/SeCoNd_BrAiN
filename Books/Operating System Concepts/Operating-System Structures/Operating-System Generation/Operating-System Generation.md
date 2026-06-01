---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.10"]
---

# Operating-System Generation
1) It is possible to design, code, and implement an operating system specifically for one machine at one site. More commonly, however, operating systems are designed to run on any of a class of machines at a variety of sites with a variety of peripheral configurations. 
2) The system must then be configured or generated for each specific computer site, a process sometimes known as **system generation (SYSGEN)**.
3) The operating system is normally distributed on disk, on CD-ROM or DVD-ROM, or as an “ISO” image, which is a file in the format of a CD-ROM or DVD-ROM. 
4) To generate a system, we use a special program. This SYSGEN program reads from a given file, or asks the operator of the system for information concerning the specific configuration of the hardware system, or probes the hardware directly to determine what components are there. The following kinds of information must be determined.
	1) What CPU is to be used? 
	2) What options (extended instruction sets, floating-point arithmetic, and so on) are installed? 
	3) For multiple CPU systems, each CPU may be described.
	4) How will the boot disk be formatted? How many sections, or “partitions,” will it be separated into, and what will go into each partition?
	5) How much memory is available? Some systems will determine this value themselves by referencing memory location after memory location until an “illegal address” fault is generated. This procedure defines the final legal address and hence the amount of available memory.
	6) What devices are available? The system will need to know how to address each device (the device number), the device interrupt number, the device’s type and model, and any special device characteristics.
	7) What operating-system options are desired, or what parameter values are to be used? These options or values might include how many buffers of which sizes should be used, what type of CPU-scheduling algorithm is desired, what the maximum number of processes to be supported is, and so on.
5) Once this information is determined, it can be used in several ways.
6) At one extreme, a system administrator can use it to modify a copy of the source code of the operating system.
7) The operating system then is completely compiled. 
8) Data declarations, initializations, and constants, along with conditional compilation, produce an output-object version of the operating system that is tailored to the system described.
9) At a slightly less tailored level, the system description can lead to the creation of tables and the selection of [[Modules|modules]] from a precompiled library. These modules are linked together to form the generated operating system. Selection allows the library to contain the device drivers for all supported I/O devices, but only those needed are linked into the operating system. Because the system is not recompiled, system generation is faster, but the resulting system may be overly general.
10) At the other extreme, it is possible to construct a system that is completely table driven. All the code is always part of the system, and selection occurs at execution time, rather than at compile or link time. System generation involves simply creating the appropriate tables to describe the system.
11) The major differences among these approaches are the size and generality of the generated system and the ease of modifying it as the hardware configuration changes. Consider the cost of modifying the system to support a newly acquired graphics terminal or another disk drive. Balanced against that cost, of course, is the frequency (or infrequency) of such changes.

## Related

- [[> Operating-System Structures]] — back to the chapter MOC
- [[System Boot]] — what happens after the system is generated
- [[Modules]] — selecting precompiled modules into the generated system
- [[Implementation]] — how the OS itself is built