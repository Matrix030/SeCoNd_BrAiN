---
tags: [book, os, operating-systems, book-os-concepts, concurrency, networking]
aliases: ["2.4.5"]
---

# Communication

1) There are two common models of interprocess communication: 
	- Message-passing model: 
		- In the **[[message passing|message-passing model]]**, the communicating [[process|processes]] exchange messages with one another to transfer information. 
		- Messages can be exchanged between the processes either directly or indirectly through a common mailbox. 
		- Before communication can take place, a connection must be opened. 
		- The name of the other communicator must be known, be it another process on the same system or a process on another computer connected by a communications network. 
		- Each computer in a network has a _host name_ by which it is commonly known. 
		- A host also has a network identifier, such as an IP address. 
		- Similarly, each process has a _process name,_ and this name is translated into an identifier by which the operating system can refer to the process. The get hostid and get processid system calls do this translation. 
		- The identifiers are then passed to the general-purpose open and close calls provided by the file system or to specific open connection and close connection system calls, depending on the system’s model of communication. 
		- The recipient process usually must give its permission for communication to take place with an accept connection call. 
		- Most processes that will be receiving connections are special-purpose _daemons,_ which are systems programs provided for that purpose. 
		- They execute a wait for connection call and are awakened when a connection is made.
		- The source of the communication, known as the _client,_ and the receiving daemon, known as a _server,_ then exchange messages by using read message and write message system calls. 
		- The close connection call terminates the communication.

	- In the **[[shared memory|shared-memory model]]**: 
		- processes use shared memory create and shared memory attach system calls to create and gain access to regions of memory owned by other processes. 
		- Recall that, normally, the operating system tries to prevent one process from accessing another process’s memory. 
		- Shared memory requires that two or more processes agree to remove this restriction. 
		- They can then exchange information by reading and writing data in the shared areas.
		- The form of the data is determined by the processes and are not under the operating system’s control. 
		- The processes are also responsible for ensuring that they are not writing to the same location simultaneously.

2) Both of the models just discussed are common in operating systems, and most systems implement both. 
	- Message passing is useful for exchanging smaller amounts of data, because no conflicts need be avoided. It is also easier to implement than is shared memory for intercomputer communication.
	- Shared memory allows maximum speed and convenience of communication, since it can be done at memory transfer speeds when it takes place within a computer. Problems exist, however, in the areas of protection and synchronization between the processes sharing memory.

## Related

- [[> Types of System Calls]] — back to the sub-topic MOC
- [[Information Maintenance]] — the previous category of system calls
- [[Protection]] — the next category of system calls
- [[message passing]] — the message-passing IPC model
- [[shared memory]] — the shared-memory IPC model
