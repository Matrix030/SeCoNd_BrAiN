---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.4.1", "Shared-Memory Systems"]
---

# Shared-Memory Systems

1) Interprocess communication using shared memory requires communicating processes to establish a region of shared memory.
2) Typically, a shared-memory region resides in the address space of the process creating the shared-memory segment. Other processes that wish to communicate using this shared-memory segment must attach it to their address space.
3) Recall that, normally, the operating system tries to prevent one process from accessing another process’s memory. Shared memory requires that two or more processes agree to remove this restriction. 
4) They can then exchange information by reading and writing data in the shared areas. The form of the data and the location are determined by these processes and are not under the operating system’s control.
5) The processes are also responsible for ensuring that they are not writing to the same location simultaneously.
6) To illustrate the concept of cooperating processes, let’s consider the producer-consumer problem, which is a common paradigm for cooperating processes. 
	1) A **producer** process produces information that is consumed by a **consumer** process. 
	2) For example, a compiler may produce assembly code, which is consumed by an assembler. 
	3) The assembler, in turn, may produce object modules, which are consumed by the loader. 
	4) The producer-consumer problem also provides a useful metaphor for the client-server paradigm.
7) One solution to the producer-consumer problem uses shared memory. To allow producer and consumer processes to run concurrently, we must have available a buffer of items that can be filled by the producer and emptied by the consumer. 
8) This buffer will reside in a region of memory that is shared by the producer and consumer processes. A producer can produce one item while the consumer is consuming another item. 
9) The producer and consumer must be synchronized, so that the consumer does not try to consume an item that has not yet been produced.
10) Two types of buffers can be used. 
	- The **unbounded buffer** places no practical limit on the size of the buffer. The consumer may have to wait for new items, but the producer can always produce new items. 
	- The **bounded buffer** assumes a fixed buffer size. In this case, the consumer must wait if the buffer is empty, and the producer must wait if the buffer is full.
11) Let’s look more closely at how the bounded buffer can be used to enable processes to share memory. The following variables reside in a region of memory shared by the producer and consumer processes:

![[Pasted image 20260602143825.png]]

13) The shared buffer is implemented as a circular array with two logical pointers: in and out. The variable in points to the next free position in the buffer; out points to the first full position in the buffer. The buffer is empty when in == out; the buffer is full when ((in + 1) % BUFFER_SIZE) == out.
14) The code for the producer and consumer processes is shown in the figures below. The producer process has a local variable nextProduced in which the new item to be produced is stored. The consumer process has a local variable nextConsumed in which the item to be consumed is stored.
15) This scheme allows at most BUFFER_SIZE—1 items in the buffer at the same time.

![[Pasted image 20260602143834.png]]

## Related

- [[> Interprocess Communication]] — back to the sub-topic MOC
- [[Cooperating Processes]] — why processes cooperate, and the two IPC models
- [[Message-Passing Systems]] — the alternative IPC model
