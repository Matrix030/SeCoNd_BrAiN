---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.9.1"]
---

# Failure Analysis

1) If a process fails, most operating systems write the error information to a **log file** to alert system operators or users that the problem occurred. 
2) The operating system can also take a core dump—a capture of the memory (referred to as the “core” in the early days of computing) of the process. This core image is stored in a file for later analysis. 
3) Running programs and core dumps can be probed by a **debugger**, a tool designed to allow a programmer to explore the code and memory of a process.
4) Debugging user-level process code is a challenge.
5) Operating system kernel debugging even more complex because of the size and complexity of the kernel, its control of the hardware, and the lack of user-level debugging tools. 
6) A kernel failure is called a **crash**. As with a process failure, error information is saved to a log file, and the memory state is saved to a **crash dump**.
7) Operating system debugging frequently uses different tools and techniques than process debugging due to the very different nature of these two tasks. Consider that a kernel failure in the file-system code would make it risky for the kernel to try to save its state to a file on the file system before rebooting. A common technique is to save the kernel’s memory state to a section of disk set aside for this purpose that contains no file system. 
8) If the kernel detects an unrecoverable error, it writes the entire contents of memory, or at least the kernel-owned parts of the system memory, to the disk area.
9) When the system reboots, a process runs to gather the data from that area and write it to a crash dump file within a file system for analysis.

## Related

- [[> Operating-System Debugging]] — back to the sub-topic MOC
- [[Performance Tuning]] — the other side of debugging
- [[DTrace]] — a tool for live failure and performance analysis
