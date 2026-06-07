---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.5.1"]
---

# Windows XP Threads

1) Windows XP implements the Win32 API, which is the primary API for the family of Microsoft operating systems (Windows 95, 98, NT, 2000, and XP). Indeed, much of what is mentioned in this section applies to this entire family of operating systems.
2) A Windows XP application runs as a separate process, and each process may contain one or more threads. The Win32 API for creating threads is covered in Section 4.3.2 (see [[Win32 Threads]]). 
3) Windows XP uses the [[One-to-One Model|one-to-one mapping]] described in Section 4.2.2, where each user-level thread maps to an associated kernel thread. However, Windows XP also provides support for a **fiber** library, which provides the functionality of the [[Many-to-Many Model|many-to-many model]] (Section 4.2.3). By using the thread library, any thread belonging to a process can access the address space of the process.
4) The general components of a thread include:
	- A thread ID uniquely identifying the thread
	- A register set representing the status of the processor
	- A user stack, employed when the thread is running in user mode, and a kernel stack, employed when the thread is running in kernel mode
	- A private storage area used by various run-time libraries and dynamic link libraries (DLLs)
5) The register set, stacks, and private storage area are known as the **context** of the thread. The primary data structures of a thread include:
	• ETHREAD—executive thread block
	• KTHREAD—kernel thread block
	• TEB—thread environment block
6) The key components of the ETHREAD include a pointer to the process to which the thread belongs and the address of the routine in which the thread starts control. The ETHREAD also contains a pointer to the corresponding KTHREAD.

![[Pasted image 20260607132934.png]]

7) The KTHREAD includes scheduling and synchronization information for the thread. In addition, the KTHREAD includes the kernel stack (used when the thread is running in kernel mode) and a pointer to the TEB.
8) The ETHREAD and the KTHREAD exist entirely in kernel space; this means that only the kernel can access them. The TEB is a user-space data structure that is accessed when the thread is running in user mode. Among other fields, the TEB contains the thread identifier, a user-mode stack, and an array for thread-specific data (which Windows XP terms **thread-local storage**). The structure of a Windows XP thread is illustrated in the figure above.

## Related

- [[> Operating-System Examples]] — back to the sub-topic MOC
- [[Linux Threads]] — the other example covered in this section
- [[Win32 Threads]] — the thread-creation API referenced here
- [[Thread-Specific Data]] — what thread-local storage provides
