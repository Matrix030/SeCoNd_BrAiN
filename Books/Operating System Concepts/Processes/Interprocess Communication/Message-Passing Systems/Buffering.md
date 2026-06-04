---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.4.2.3", "Buffering"]
---

# Buffering

1) Whether communication is direct or indirect, messages exchanged by communicating processes reside in a temporary queue. Basically, such queues can be implemented in three ways:
	- **Zero capacity**. The queue has a maximum length of zero; thus, the link cannot have any messages waiting in it. In this case, the sender must block until the recipient receives the message.
	- **Bounded capacity**. The queue has finite length _n;_ thus, at most _n_ messages can reside in it. If the link is full, the sender must block until space is available in the queue.
	- **Unbounded capacity**. The queue’s length is potentially infinite; thus, any number of messages can wait in it. The sender never blocks.
2) The zero-capacity case is sometimes referred to as a message system with no buffering; the other cases are referred to as systems with automatic buffering.

## Related

- [[> Message-Passing Systems]] — back to the section MOC
- [[Naming]] — direct vs indirect communication
- [[Synchronization]] — blocking vs nonblocking send/receive
