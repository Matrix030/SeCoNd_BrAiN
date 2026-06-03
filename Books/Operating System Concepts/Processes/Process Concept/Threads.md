---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.1.4"]
---

# Threads

1) The process model discussed so far has implied that a process is a program that performs a single **thread** of execution.
2) For example, when a process is running a word-processor program, a single thread of instructions is being executed.
3) This single thread of control allows the process to perform only one task at one time. 
4) The user cannot simultaneously type in characters and run the spell checker within the same process, for example. 
5) Many modern operating systems have extended the process concept to allow a process to have multiple threads of execution and thus to perform more than one task at a time. 
6) On a system that supports threads, the PCB is expanded to include information for each thread. 
7) Other changes throughout the system are also needed to support threads.

![[Pasted image 20260601204541.png]]

## Related

- [[> Process Concept]] — back to the sub-topic MOC
- [[The Process]] — the single-threaded process model
- [[Process Control Block]] — expanded to hold per-thread information
