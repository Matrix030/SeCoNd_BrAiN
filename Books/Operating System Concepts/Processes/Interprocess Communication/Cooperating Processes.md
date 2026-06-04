---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["Cooperating Processes", "Independent Processes"]
---

# Cooperating Processes

1) Processes executing concurrently in the operating system may be either independent processes or cooperating processes. A process is **independent** if it cannot affect or be affected by the other processes executing in the system. 
2) Any process that does not share data with any other process is independent.
3) A process is **cooperating** if it can affect or be affected by the other processes executing in the system. Clearly, any process that shares data with other processes is a cooperating process.
4) There are several reasons for providing an environment that allows process cooperation:
	- **Information sharing**. Since several users may be interested in the same piece of information (for instance, a shared file), we must provide an environment to allow concurrent access to such information.
	- **Computation speedup**. If we want a particular task to run faster, we must break it into subtasks, each of which will be executing in parallel with the others. Notice that such a speedup can be achieved only if the computer has multiple processing elements (such as CPUs or I/O channels).
	- **Modularity**. We may want to construct the system in a modular fashion, dividing the system functions into separate processes or threads, as we discussed in Chapter 2.
	- **Convenience**. Even an individual user may work on many tasks at the same time. For instance, a user may be editing, printing, and compiling in parallel.
5) Cooperating processes require an **interprocess communication** (**IPC**) mechanism that will allow them to exchange data and information. 
6) There are two fundamental models of interprocess communication: 
	- (1) **shared memory** - a region of memory that is shared by cooperating processes is established. Processes can then exchange information by reading and writing data to the shared region. (see [[Shared-Memory Systems]])
	- (2) **message passing** - In the message-passing model, communication takes place by means of messages exchanged between the cooperating processes. (see [[Message-Passing Systems]])
7) The two communications models are contrasted in the figure below.
![[Pasted image 20260602143811.png]]

8) Message passing is useful for exchanging smaller amounts of data, because no conflicts need be avoided. Message passing is also easier to implement than is shared memory for intercomputer communication. 
9) Shared memory allows maximum speed and convenience of communication. Shared memory is faster than message passing, as message-passing systems are typically implemented using system calls and thus require the more time-consuming task of kernel intervention. 
10) In contrast, in shared-memory systems, system calls are required only to establish shared-memory regions. Once shared memory is established, all accesses are treated as routine memory accesses, and no assistance from the kernel is required.

## Related

- [[> Interprocess Communication]] — back to the sub-topic MOC
- [[Shared-Memory Systems]] — the first IPC model, in detail
- [[Message-Passing Systems]] — the second IPC model, in detail
