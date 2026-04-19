---
tags: [system-design, hld, networking]
aliases: ["OSI Model", "OSI Layers", "Networking Layers"]
---

# Networking Layers

![[Pasted image 20260418001307.png]]

###### Network Layer (Layer 3)

1) At this layer is IP, the protocol that handles routing and addressing.
2) It's responsible for breaking the data into packets, handling packet forwarding between networks, and providing best-effort delivery to any destination IP address on the network.
3) While there are other protocols at this layer (like [InfiniBand](https://en.wikipedia.org/wiki/InfiniBand), which is used extensively for massive ML training workloads), IP by far the most common for system design interviews.

###### Transport Layer (Layer 4)

1) At this layer, we have [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol), [QUIC](https://en.wikipedia.org/wiki/QUIC), and [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol), which provide end-to-end communication services.
2) Think of them like a layer that provides features like reliability, ordering, and flow control on top of the network layer.

###### Application Layer (Layer 7)

1) At the final layer are the application protocols like DNS, HTTP, Websockets, WebRTC.
2) These are common protocols that build on top of TCP (or UDP, in the case of WebRTC) to provide a layer of abstraction for different types of data typically associated with web applications.
3) These layers work together to enable all our network communications.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[A Simple Web Request]] — see these layers working together end-to-end
- [[Network Layer (IP)]] — deep dive on Layer 3
- [[Transport Layer Protocols]] — deep dive on Layer 4
- [[HTTP and HTTPS]] — key Layer 7 protocol
