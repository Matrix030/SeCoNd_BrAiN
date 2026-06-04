---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.4.2.1", "Naming"]
---

# Naming

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
6) With **indirect communication**, the messages are sent to and received from **mailboxes**, or **ports**. A mailbox can be viewed abstractly as an object into which messages can be placed by processes and from which messages can be removed. Each mailbox has a unique identification. For example, POSIX message queues use an integer value to identify a mailbox. In this scheme, a process can communicate with some other process via a number of different mailboxes. Two processes can communicate only if the processes have a shared mailbox, however. The send() and receive() primitives are defined as follows:
	- send(A, message)—Send a message to mailbox A.
	- receive(A, message)—Receive a message from mailbox A.
7) A communication link in this scheme has the following properties:
	- A link is established between a pair of processes only if both members of the pair have a shared mailbox.
	- A link may be associated with more than two processes.
	- Between each pair of communicating processes, there may be a number of different links, with each link corresponding to one mailbox.
8) Now suppose that processes _P_1, _P_2, and _P_3 all share mailbox _A_. Process _P_1 sends a message to _A_, while both _P_2 and _P_3 execute a receive() from _A_. Which process will receive the message sent by _P_1? The answer depends on which of the following methods we choose:
	- Allow a link to be associated with two processes at most.
	- Allow at most one process at a time to execute a receive() operation.
	- Allow the system to select arbitrarily which process will receive the message (that is, _round robin,_ where processes take turns receiving messages).
9) A mailbox may be owned either by a process or by the operating system. If the mailbox is owned by a process, then we distinguish between the **owner** (which can only receive messages through this mailbox) and the **user** (which can only send messages to the mailbox). When a process that owns a mailbox terminates, the mailbox disappears.
10) In contrast, a mailbox that is owned by the operating system has an existence of its own. It is independent and is not attached to any particular process. The operating system then must provide a mechanism that allows a process to do the following:
	- Create a new mailbox.
	- Send and receive messages through the mailbox.
	- Delete a mailbox.
11) The process that creates a new mailbox is that mailbox’s owner by default. Initially, the owner is the only process that can receive messages through this mailbox. However, the ownership and receiving privilege may be passed to other processes through appropriate system calls.

## Related

- [[> Message-Passing Systems]] — back to the section MOC
- [[Synchronization]] — blocking vs nonblocking send/receive
- [[Buffering]] — the queue that backs a communication link
