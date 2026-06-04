---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.4.2", "Message-Passing Systems"]
---

# Message-Passing Systems

1) In Section 3.4.1, we showed how cooperating processes can communicate in a shared-memory environment. The scheme requires that these processes share a region of memory and that the code for accessing and manipulating the shared memory be written explicitly by the application programmer. Another way to achieve the same effect is for the operating system to provide the means for cooperating processes to communicate with each other via a message-passing facility.
2) Message passing provides a mechanism to allow processes to communicate and to synchronize their actions without sharing the same address space and is particularly useful in a distributed environment, where the communicating processes may reside on different computers connected by a network. For example, a **chat** program used on the World Wide Web could be designed so that chat participants communicate with one another by exchanging messages.

![[Pasted image 20260602143844.png]]

3) A message-passing facility provides at least two operations: 
	- send(message) and 
	- receive(message). 
4) Messages sent by a process can be of either fixed or variable size. If only fixed-sized messages can be sent, the system-level implementation is straightforward. 
5) This restriction, however, makes the task of programming more difficult. Conversely, variable-sized messages require a more complex system-level implementation, but the programming task becomes simpler. 
6) This is a common kind of tradeoff seen throughout operating-system design.
7) If processes _P_ and _Q_ want to communicate, they must send messages to and receive messages from each other; a **communication link** must exist between them. 
8) This link can be implemented in a variety of ways. We are concerned here not with the link’s physical implementation (such as shared memory, hardware bus, or network, which are covered in Chapter 16) but rather with its logical implementation. Here are several methods for logically implementing a link and the send()/receive() operations:
	- Direct or indirect communication (see [[Naming]])
	- Synchronous or asynchronous communication (see [[Synchronization]])
	- Automatic or explicit buffering (see [[Buffering]])

We look at issues related to each of these features next.

## Related

- [[> Message-Passing Systems]] — back to the section MOC
- [[> Interprocess Communication]] — back to the sub-topic MOC
- [[Shared-Memory Systems]] — the alternative IPC model
- [[Cooperating Processes]] — why processes cooperate, and the two IPC models
