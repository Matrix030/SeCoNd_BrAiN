---
tags: [system-design, hld, networking, tcp]
aliases: ["Transmission Control Protocol", "TCP Protocol"]
---

# TCP: Reliable but with Overhead

1) Transmission Control Protocol (TCP) is the workhorse of the internet.
2) It provides reliable, ordered, and error-checked delivery of data.
3) It establishes a connection through a three-way handshake (we saw this illustrated above with the HTTP example) and maintains that connection throughout the communication session.
4) This connection is called a "stream" and is a **stateful connection** between the client and server — it also gives us a basis to talk about ordering: two messages sent in the same stream/connection will arrive in the same order.
5) TCP will ensure that recipients of messages acknowledge their receipt and, if they don't, will retransmit the message until it is acknowledged.

#### Key Characteristics of TCP

1. **Connection-oriented**: Establishes a dedicated connection before data transfer
2. **Reliable delivery**: Guarantees that data arrives in order and without errors
3. **Flow control**: Prevents overwhelming receivers with too much data
4. **Congestion control**: Adapts to network congestion to prevent collapse

TCP is ideal for applications where data integrity is critical — that is, **basically everything where UDP is not a good fit**.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[UDP]] — the fast, unreliable alternative
- [[Transport Layer Protocols]] — when to choose TCP vs UDP
- [[A Simple Web Request]] — TCP three-way handshake in action
- [[WebSockets]] — persistent TCP connections for bidirectional comms
