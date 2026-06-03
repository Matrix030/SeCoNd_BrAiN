---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.1.1"]
---

# The Process

1) Informally, a process is a program in execution. 
2) A process is more than the program code, which is sometimes known as the **text section** (program code).
3) It also includes the current activity, as represented by the value of the **program counter** and the contents of the processor’s registers. 
4) A process generally also includes the process **stack**, which contains temporary data (such as function parameters, return addresses, and local variables), and a **data section**, which contains global variables.
5) A process may also include a **heap**, which is memory that is dynamically allocated during process run time. The structure of a process in memory is shown in the figure below.

![[Pasted image 20260601204500.png]]

6) We emphasize that a program by itself is not a process; a program is a _passive_ entity, such as a file containing a list of instructions stored on disk (often called an **executable file**), whereas a process is an _active_ entity, with a program counter specifying the next instruction to execute and a set of associated resources.
7) Although two processes may be associated with the same program, they are nevertheless considered two separate execution sequences, although the text sections are equivalent, the data, heap, and stack sections vary. 
8) It is also common to have a process that spawns many processes as it runs.

## Related

- [[> Process Concept]] — back to the sub-topic MOC
- [[Process State]] — the states a process moves through as it executes
- [[Process Control Block]] — how the OS represents a process
- [[Threads]] — a process with multiple threads of execution
