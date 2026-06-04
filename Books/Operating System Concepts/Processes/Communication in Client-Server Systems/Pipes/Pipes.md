---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.6.3", "Pipes"]
---

# Pipes

1) A **pipe** acts as a conduit allowing two processes to communicate. Pipes were one of the first IPC mechanisms in early UNIX systems and typically provide one of the simpler ways for processes to communicate with one another, although they also have some limitations. In implementing a pipe, four issues must be considered:
	1. Does the pipe allow unidirectional communication or bidirectional communication?
	2. If two-way communication is allowed, is it half duplex (data can travel only one way at a time) or full duplex (data can travel in both directions at the same time)?
	3. Must a relationship (such as _parent-child_) exist between the communicating processes?
	4. Can the pipes communicate over a network, or must the communicating processes reside on the same machine?
2) In the following sections, we explore two common types of pipes used on both UNIX and Windows systems.

## Related

- [[> Pipes]] — back to the section MOC
- [[> Communication in Client-Server Systems]] — back to the sub-topic MOC
- [[Ordinary Pipes]] — unidirectional pipes with a parent-child relationship
- [[Named Pipes]] — bidirectional, persistent pipes with no parent-child requirement
