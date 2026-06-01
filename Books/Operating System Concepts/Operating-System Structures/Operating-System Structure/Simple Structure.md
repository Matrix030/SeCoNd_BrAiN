---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.7.1"]
---

# Simple Structure

1) Another example of limited structuring is the original UNIX operating system. Like MS-DOS, UNIX initially was limited by hardware functionality. It consists of two separable parts: the kernel and the system programs. 
2) The kernel is further separated into a series of interfaces and device drivers, which have been added and expanded over the years as UNIX has evolved. 
3) We can view the traditional UNIX operating system as being layered, as shown in Figure 2.13. Everything below the system-call interface and above the physical hardware is the [[kernel]]. The kernel provides the file system, CPU scheduling, memory management, and other operating-system functions through [[> System Calls|system calls]]. 
4) Taken in sum, that is an enormous amount of functionality to be combined into one level. This monolithic structure was difficult to implement and maintain.

![[Pasted image 20260529212324.png]]

![[Pasted image 20260529212330.png]]

## Related

- [[> Operating-System Structure]] — back to the sub-topic MOC
- [[Layered Approach]] — the more structured alternative
- [[Microkernels]] — the opposite extreme of a minimal kernel
