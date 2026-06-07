---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.3"]
---

# Thread Library Implementation

1) A **thread library** provides the programmer with an API for creating and managing threads. There are two primary ways of implementing a thread library. The first approach is to provide a library entirely in user space with no kernel support. All code and data structures for the library exist in user space. This means that invoking a function in the library results in a local function call in user space and not a system call. (long way of saying API)
2) The second approach is to implement a kernel-level library supported directly by the operating system. In this case, code and data structures for the library exist in kernel space. Invoking a function in the API for the library typically results in a system call to the kernel.
3) Three main thread libraries are in use today: 
	1) POSIX Pthreads, 
	2) Win32
	3) Java.
4) Pthreads, the threads extension of the POSIX standard, may be provided as either a user- or kernel-level library. The Win32 thread library is a kernel-level library available on Windows systems.
5) The Java thread API allows threads to be created and managed directly in Java programs. However, because in most instances the JVM is running on top of a host operating system, the Java thread API is generally implemented using a thread library available on the host system.
6) This means that on Windows systems, Java threads are typically implemented using the Win32 API; UNIX and Linux systems often use Pthreads.
7) In the remainder of this section, we describe basic thread creation using these three thread libraries. As an illustrative example, we design a multithreaded program that performs the summation of a non-negative integer in a separate thread using the well-known summation function:

$$sum = \sum_{i=0}^{N} i$$

8) For example, if _N_ were 5, this function would represent the summation of integers from 0 to 5, which is 15. Each of the three programs will be run with the upper bounds of the summation entered on the command line; thus, if the user enters 8, the summation of the integer values from 0 to 8 will be output.

## Related

- [[> Thread Libraries]] — back to the sub-topic MOC
- [[Pthreads]] — the POSIX thread library
- [[Win32 Threads]] — the Windows kernel-level thread library
- [[Java Threads]] — threads in the JVM
- [[User and Kernel Threads]] — the user-space vs kernel-space distinction this builds on
