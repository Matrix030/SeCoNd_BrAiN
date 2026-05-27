---
tags: [book, os, operating-systems, book-os-concepts, memory]
aliases: ["Memory Management"]
---

# Memory Management

1) The main memory is central to the operation of a modern computer system. Main memory is a large array of words or bytes, ranging in size from hundreds of thousands to billions.
2) Each word or byte has its own address. Main memory is a repository of quickly accessible data shared by the CPU and I/O devices. The central processor reads instructions from main memory during the instruction-fetch cycle and both reads and writes data from main memory during the data-fetch cycle (on a von Neumann architecture).
3) As noted earlier, the main memory is generally the only large storage device that the CPU is able to address and access directly. For example, for the CPU to process data from disk, those data must first be transferred to main memory by CPU-generated I/O calls. In the same way, instructions must be in memory for the CPU to execute them.
4) For a program to be executed, it must be mapped to absolute addresses and loaded into memory. As the program executes, it accesses program instructions and data from memory by generating these absolute addresses. Eventually, the program terminates, its memory space is declared available, and the next program can be loaded and executed.
5) To improve both the utilization of the CPU and the speed of the computer’s response to its users, general-purpose computers must keep several programs in memory, creating a need for memory management. Many different memory-management schemes are used. These schemes reflect various approaches, and the effectiveness of any given algorithm depends on the situation. In selecting a memory-management scheme for a specific system, we must take into account many factors—especially the _hardware_ design of the system. Each algorithm requires its own hardware support.

The operating system is responsible for the following activities in connection with memory management:
• Keeping track of which parts of memory are currently being used and by whom
• Deciding which processes (or parts thereof) and data to move into and out of memory
• Allocating and deallocating memory space as needed

## Related

- [[> Introduction]] — back to the chapter MOC
- [[> Process Management]] — the processes kept in memory together
- [[> Storage Management]] — the storage hierarchy main memory sits within
