---
tags: [book, os, operating-systems, book-os-concepts, processes, concurrency]
aliases: ["3.4.2.2", "Synchronization"]
---

# Synchronization

1) Communication between processes takes place through calls to send() and receive() primitives. Message passing may be either blocking or nonblocking—also known as **synchronous** and **asynchronous**.
	- **Blocking send**. The sending process is blocked until the message is received by the receiving process or by the mailbox.
	- **Nonblocking send**. The sending process sends the message and resumes operation.
	- **Blocking receive**. The receiver blocks until a message is available.
	- **Nonblocking receive**. The receiver retrieves either a valid message or a null.
2) When both send() and receive() are blocking, we have a **rendezvous** between the sender and the receiver. The solution to the producer-consumer problem becomes trivial when we use blocking send() and receive() statements.

## Related

- [[> Message-Passing Systems]] — back to the section MOC
- [[Naming]] — direct vs indirect communication
- [[Buffering]] — how queued messages are held between send and receive
