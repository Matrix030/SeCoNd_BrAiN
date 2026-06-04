---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["Processes Summary"]
---

# Processes Summary

1) A process is a program in execution. As a process executes, it changes state. The state of a process is defined by that process’s current activity. Each process may be in one of the following states: new, ready, running, waiting, or terminated. Each process is represented in the operating system by its own process control block (PCB).
2) A process, when it is not executing, is placed in some waiting queue. There are two major classes of queues in an operating system: I/O request queues and the ready queue. The ready queue contains all the processes that are ready to execute and are waiting for the CPU. Each process is represented by a PCB, and the PCBs can be linked together to form a ready queue. Long-term (job) scheduling is the selection of processes that will be allowed to contend for the CPU. Normally, long-term scheduling is heavily influenced by resource-allocation considerations, especially memory management. Short-term (CPU) scheduling is the selection of one process from the ready queue.
3) Operating systems must provide a mechanism for parent processes to create new child processes. The parent may wait for its children to terminate before proceeding, or the parent and children may execute concurrently. There are several reasons for allowing concurrent execution: information sharing, computation speedup, modularity, and convenience.
4) The processes executing in the operating system may be either independent processes or cooperating processes. Cooperating processes require an interprocess communication mechanism to communicate with each other. Principally, communication is achieved through two schemes: shared memory and message passing. The shared-memory method requires communicating processes to share some variables. The processes are expected to exchange information through the use of these shared variables. In a shared-memory system, the responsibility for providing communication rests with the application programmers; the operating system needs to provide only the shared memory. The message-passing method allows the processes to exchange messages. The responsibility for providing communication may rest with the operating system itself. These two schemes are not mutually exclusive and can be used simultaneously within a single operating system.
5) Communication in client - server systems may use (1) sockets, (2) remote procedure calls (RPCs), or (3) pipes. A socket is defined as an endpoint for communication. A connection between a pair of applications consists of a pair of sockets, one at each end of the communication channel. RPCs are another form of distributed communication. An RPC occurs when a process (or thread) calls a procedure on a remote application. Ordinary pipes allow communication between parent and child processes, while named pipes permit unrelated processes to communicate with one another.

## Related

- [[> Processes]] — back to the chapter MOC
- [[> Process Concept]] — process states and the process control block (PCB)
- [[> Process Scheduling]] — ready/waiting queues and long-term vs short-term scheduling
- [[> Operations on Processes]] — parent and child process creation
- [[> Interprocess Communication]] — shared memory and message passing
- [[> Communication in Client-Server Systems]] — sockets, RPCs, and pipes
