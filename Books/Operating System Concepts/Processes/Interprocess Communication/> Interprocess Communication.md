---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["Interprocess Communication", "IPC"]
---

# Interprocess Communication — Map of Content

> [!info] Section 3.4
> Drop raw notes for this sub-topic here. Once filled, this file is processed into leaf notes and rewritten as the index.

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
	- (1) **shared memory** - a region of memory that is shared by cooperating processes is established. Processes can then exchange information by reading and writing data to the shared region. 
	- (2) **message passing** - In the message-passing model, communication takes place by means of messages exchanged between the cooperating processes. 
7) The two communications models are contrasted in the figure below.
![[Pasted image 20260602143811.png]]

8) Message passing is useful for exchanging smaller amounts of data, because no conflicts need be avoided. Message passing is also easier to implement than is shared memory for intercomputer communication. 
9) Shared memory allows maximum speed and convenience of communication. Shared memory is faster than message passing, as message-passing systems are typically implemented using system calls and thus require the more time-consuming task of kernel intervention. 
10) In contrast, in shared-memory systems, system calls are required only to establish shared-memory regions. Once shared memory is established, all accesses are treated as routine memory accesses, and no assistance from the kernel is required.

## **3.4.1 Shared-Memory Systems**
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

## **3.4.2 Message-Passing Systems**
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
	- Direct or indirect communication
	- Synchronous or asynchronous communication
	- Automatic or explicit buffering

We look at issues related to each of these features next.

### **3.4.2.1 Naming**
1) Processes that want to communicate must have a way to refer to each other. They can use either direct or indirect communication.
2) Under **direct communication**, each process that wants to communicate must explicitly name the recipient or sender of the communication. In this scheme, the send() and receive() primitives are defined as:
	- send (P, message)—Send a message to process P.
	- receive (Q, message)—Receive a message from process Q.

3) A communication link in this scheme has the following properties:
	- A link is established automatically between every pair of processes that want to communicate. The processes need to know only each other’s identity to communicate.
	- A link is associated with exactly two processes.
	- Between each pair of processes, there exists exactly one link.
4) This scheme exhibits _symmetry_ in addressing; that is, both the sender process and the receiver process must name the other to communicate. A variant of this scheme employs _asymmetry_ in addressing. Here, only the sender names the recipient; the recipient is not required to name the sender. In this scheme, the send() and receive() primitives are defined as follows:
	- send (P, message)—Send a message to process P.
	- receive(id, message)—Receive a message from any process; the variable _id_ is set to the name of the process with which communication has taken place.
5) The disadvantage in both of these schemes (symmetric and asymmetric) is the limited modularity of the resulting process definitions. Changing the identifier of a process may necessitate examining all other process definitions. All references to the old identifier must be found, so that they can be modified to the new identifier. In general, any such **hard-coding** techniques, where identifiers must be explicitly stated, are less desirable than techniques involving indirection, as described next.

With **indirect communication**, the messages are sent to and received from **mailboxes**, or **ports**. A mailbox can be viewed abstractly as an object into which messages can be placed by processes and from which messages can be removed. Each mailbox has a unique identification. For example, POSIX message queues use an integer value to identify a mailbox. In this scheme, a process can communicate with some other process via a number of different mailboxes. Two processes can communicate only if the processes have a shared mailbox, however. The send() and receive() primitives are defined as follows:

• send(A, message)—Send a message to mailbox A.

• receive(A, message)—Receive a message from mailbox A.

In this scheme, a communication link has the following properties:

• A link is established between a pair of processes only if both members of the pair have a shared mailbox.

• A link may be associated with more than two processes.

• Between each pair of communicating processes, there may be a number of different links, with each link corresponding to one mailbox.

Now suppose that processes _P_1, _P_2, and _P_3 all share mailbox _A_. Process _P_1 sends a message to _A_, while both _P_2 and _P_3 execute a receive() from _A_. Which process will receive the message sent by _P_1? The answer depends on which of the following methods we choose:

• Allow a link to be associated with two processes at most.

• Allow at most one process at a time to execute a receive() operation.

• Allow the system to select arbitrarily which process will receive the message (that is, either _P_2 or _P_3, but not both, will receive the message). The system also may define an algorithm for selecting which process will receive the message (that is, _round robin,_ where processes take turns receiving messages). The system may identify the receiver to the sender.

A mailbox may be owned either by a process or by the operating system. If the mailbox is owned by a process (that is, the mailbox is part of the address space of the process), then we distinguish between the owner (which can only receive messages through this mailbox) and the user (which can only send messages to the mailbox). Since each mailbox has a unique owner, there can be no confusion about which process should receive a message sent to this mailbox. When a process that owns a mailbox terminates, the mailbox disappears. Any process that subsequently sends a message to this mailbox must be notified that the mailbox no longer exists.

In contrast, a mailbox that is owned by the operating system has an existence of its own. It is independent and is not attached to any particular process. The operating system then must provide a mechanism that allows a process to do the following:

• Create a new mailbox.

• Send and receive messages through the mailbox.

• Delete a mailbox.

The process that creates a new mailbox is that mailbox’s owner by default. Initially, the owner is the only process that can receive messages through this mailbox. However, the ownership and receiving privilege may be passed to other processes through appropriate system calls. Of course, this provision could result in multiple receivers for each mailbox.

### **3.4.2.2 Synchronization**

Communication between processes takes place through calls to send() and receive() primitives. There are different design options for implementing each primitive. Message passing may be either blocking or nonblocking—also known as **synchronous** and **asynchronous**.

• **Blocking send**. The sending process is blocked until the message is received by the receiving process or by the mailbox.

• **Nonblocking send**. The sending process sends the message and resumes operation.

• **Blocking receive**. The receiver blocks until a message is available.

• **Nonblocking receive**. The receiver retrieves either a valid message or a null.

Different combinations of send() and receive() are possible. When both send() and receive() are blocking, we have a **rendezvous** between the sender and the receiver. The solution to the producer-consumer problem becomes trivial when we use blocking send() and receive() statements. The producer merely invokes the blocking send() call and waits until the message is delivered to either the receiver or the mailbox. Likewise, when the consumer invokes receive(), it blocks until a message is available.

Note that the concepts of synchronous and asynchronous occur frequently in operating-system I/O algorithms, as you will see throughout this text.

### **3.4.2.3 Buffering**

Whether communication is direct or indirect, messages exchanged by communicating processes reside in a temporary queue. Basically, such queues can be implemented in three ways:

• **Zero capacity**. The queue has a maximum length of zero; thus, the link cannot have any messages waiting in it. In this case, the sender must block until the recipient receives the message.

• **Bounded capacity**. The queue has finite length _n;_ thus, at most _n_ messages can reside in it. If the queue is not full when a new message is sent, the message is placed in the queue (either the message is copied or a pointer to the message is kept), and the sender can continue execution without waiting. The link’s capacity is finite, however. If the link is full, the sender must block until space is available in the queue.

• **Unbounded capacity**. The queue’s length is potentially infinite; thus, any number of messages can wait in it. The sender never blocks.

The zero-capacity case is sometimes referred to as a message system with no buffering; the other cases are referred to as systems with automatic buffering.


---

## Related

- [[> Processes]] — back to the chapter MOC
