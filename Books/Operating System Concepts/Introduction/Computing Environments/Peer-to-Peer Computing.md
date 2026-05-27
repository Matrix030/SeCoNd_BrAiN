---
tags: [book, os, operating-systems, book-os-concepts, networking]
aliases: ["Peer-to-Peer Computing", "P2P"]
---

# Peer-to-Peer Computing

1) Another structure for a distributed system is the peer-to-peer (P2P) system model. In this model, clients and servers are not distinguished from one another; instead, all nodes within the system are considered peers, and each may act as either a client or a server, depending on whether it is requesting or providing a service. 
2) Peer-to-peer systems offer an advantage over traditional client-server systems. In a client-server system, the server is a bottleneck; but in a peer-to-peer system, services can be provided by several nodes distributed throughout the network.
3) To participate in a peer-to-peer system, a node must first join the network of peers. Once a node has joined the network, it can begin providing services to—and requesting services from—other nodes in the network. Determining what services are available is accomplished in one of two general ways:
	- When a node joins a network, it registers its service with a **centralized lookup service** on the network. Any node desiring a specific service first contacts this centralized lookup service to determine which node provides the service. The remainder of the communication takes place between the client and the service provider.
	- A peer acting as a client must first discover what node provides a desired service by broadcasting a request for the service to all other nodes in the network. The node (or nodes) providing that service responds to the peer making the request. To support this approach, a _discovery protocol_ must be provided that allows peers to discover services provided by other peers in the network.
4) Peer-to-peer networks gained widespread popularity in the late 1990s with several file-sharing services, such as Napster and Gnutella, that enable peers to exchange files with one another.

## Related

- [[> Computing Environments]] — back to the sub-topic MOC
- [[Client-Server Computing]] — the model P2P contrasts with
- [[> Distributed Systems]] — P2P is another distributed system structure
