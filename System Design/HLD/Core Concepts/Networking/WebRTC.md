---
tags: [system-design, hld, networking, real-time, webrtc, udp]
aliases: ["WebRTC", "Peer-to-Peer", "P2P Communication"]
---

# WebRTC: Peer-to-Peer Communication

1) WebRTC enables direct **peer-to-peer** communication between browsers without requiring an intermediary server for the data exchange.
2) WebRTC can be perfect for collaborative applications like document editors and is especially useful for video/audio calling and conferencing applications.
3) The WebRTC spec is comprised of several pieces of infra and protocols that are necessary to establish a peer-to-peer connection between browsers.
4) From a networking perspective, peer-to-peer connections are more complex than the client-server models we've been discussing so far because most clients don't allow inbound connections for security reasons.
5) With WebRTC, clients talk to a central **"signaling server"** which keeps track of which peers are available together with their connection information.
6) Once a client has the connection information for another peer, they can try to establish a direct connection without going through any intermediary servers.
7) In practice, most clients don't allow inbound connections for security reasons and the majority of users are behind a NAT (network address translation) device which keeps them from being connected to directly. So if we stopped there, most peers wouldn't be able to "speak" to each other.

The WebRTC standard includes two methods to work around these restrictions:

- **STUN**: "Session Traversal Utilities for NAT" is a protocol and a set of techniques like "hole punching" which allows peers to establish publically routable addresses and ports. As hacky as it sounds it's a standard way to deal with NAT traversal and it involves repeatedly creating open ports and sharing them via the signaling server with peers.
- **TURN**: "Traversal Using Relays around NAT" is effectively a relay service, a way to bounce requests through a central server which can then be routed to the appropriate peer.

![[Pasted image 20260418210720.png]]

There's effectively **4 steps to a WebRTC connection**:

1. Clients connect to a central signaling server to learn about their peers.
2. Clients reach out to a STUN server to get their public IP address and port.
3. Clients share this information with each other via the signaling server.
4. Clients establish a direct peer-to-peer connection and start sending data.

This is the happy case! In reality, sometimes these connections fail and you need to have fallbacks like our TURN server.

##### Where to Use It

1) WebRTC is ideal for audio/video calling and conferencing applications.
2) It can also occasionally be appropriate for collaborative applications like document editors, especially if they need to scale to many clients.
3) In practice, most collaborative editors _don't_ require scaling to thousands of clients.
4) Additionally, you often need a central server anyways to store the document and coordinate between clients.
5) But there is an alternative to use WebRTC and CRDTs (Conflict-free Replicated Data Types) to achieve a truly peer-to-peer experience.

Suggested sticking to WebRTC for video/audio calling and conferencing applications.

> [!warning]
> WebRTC is an absolute pain to get right and even the best implementations still suffer connection losses. It truly is a niche solution.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[UDP]] — WebRTC uses UDP under the hood for audio/video streams
- [[WebSockets]] — server-mediated alternative for real-time bidirectional comms
- [[Server-Sent Events]] — simpler push alternative for non-P2P use cases
