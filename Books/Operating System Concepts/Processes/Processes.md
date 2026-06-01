---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["Chapter 3 Processes"]
---
# Processes
1) Early computer systems allowed only one program to be executed at a time. This program had complete control of the system and had access to all the system’s resources. 
2) In contrast, current-day computer systems allow multiple programs to be loaded into memory and executed concurrently. 
3) This evolution required firmer control and more compartmentalization of the various programs; and these needs resulted in the notion of a **process**, which is a program in execution. 
4) **A process is the unit of work in a modern time-sharing system.**
5) The more complex the operating system is, the more it is expected to do on behalf of its users. Although its main concern is the execution of user programs, it also needs to take care of various system tasks that are better left outside the kernel itself.
6) A system therefore consists of a collection of processes: operating-system processes executing system code and user processes executing user code. 
7) Potentially, all these processes can execute concurrently, with the CPU (or CPUs) multiplexed among them. 
8) By switching the CPU between processes, the operating system can make the computer more productive. 
9) In this chapter, you will read about what processes are and how they work.

**CHAPTER OBJECTIVES**
- To introduce the notion of a process—a program in execution, which forms the basis of all computation.
- To describe the various features of processes, including scheduling, creation and termination, and communication.
- To describe communication in client-server systems.