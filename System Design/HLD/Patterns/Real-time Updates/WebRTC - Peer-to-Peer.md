---
tags: [system-design, hld, real-time, webrtc]
aliases: ["WebRTC Peer-to-Peer", "WebRTC for Real-time"]
---

# WebRTC: The Peer-to-Peer Solution
1) Our last option is the most unique. 
2) WebRTC enables direct peer-to-peer communication between browsers, perfect for video/audio calls and some data sharing like document editors.
3) Clients talk to a central "signaling server" which keeps track of which peers are available together with their connection information. 
4) Once a client has the connection information for another peer, they can try to establish a direct connection without going through any intermediary servers.
5) In practice, most clients don't allow inbound connections for security reasons (the exception would be servers which broadcast their availability on specific ports at specific addresses) using devices like NAT (network address translation).
6) So if we stopped there, most peers wouldn't be able to "speak" to each other.

The WebRTC standard includes two methods to work around these restrictions:

- **STUN**: 
	- "Session Traversal Utilities for NAT" is a protocol and a set of techniques like "hole punching" which allows peers to establish publically routable addresses and ports.
	- It's a standard way to deal with NAT traversal and it involves repeatedly creating open ports and sharing them via the signaling server with peers.
- **TURN**: 
	- "Traversal Using Relays around NAT" is effectively a relay service, a way to bounce requests through a central server which can then be routed to the appropriate peer.
![[Pasted image 20260524191902.png]]

1) In practice, the signaling server is relatively lightweight and isn't handling much of the bandwidth as the bulk of the traffic is handled by the peer-to-peer connections.
2) But interestingly the signaling server does effectively act as a real-time update system for its clients (so they can find their peers and update their connection info) so it either needs to utilize WebSockets, SSE, or some other approach detailed above.
3) For our chat app, we'd connect to our signaling server over a WebSocket connection to find all of our peers (others in the chat room). 
4) Once we'd identified them and exchanged connection information, we'd be able to establish direct peer-to-peer connections with them.
5) Chat messages would be broadcast by room participants to all of their peers (or, if you want to be extra fancy, bounced between participants until they settle).
## How WebRTC Works
1. Peers discover each other through signaling server.
2. Exchange connection info (ICE candidates)
3. Establish direct peer connection, using STUN/TURN if needed
4. Stream audio/video or send data directly

```
// Simplified WebRTC setup
async function startCall() {
  const pc = new RTCPeerConnection();
  
  // Get local stream
  const stream = await navigator.mediaDevices.getUserMedia({
    video: true,
    audio: true
  });
  
  // Add tracks to peer connection
  stream.getTracks().forEach(track => {
    pc.addTrack(track, stream);
  });
  
  // Create and send offer
  const offer = await pc.createOffer();
  await pc.setLocalDescription(offer);
  
  // Send offer to signaling server
  signalingServer.send(offer);
}
```

## When to Use WebRTC
1) WebRTC is the most complex and heavyweight of the options we've discussed. 
2) It's overkill for most real-time update use cases, but it's a great tool for scenarios like video/audio calls, screen sharing, and gaming.
3) The notable exception is that it can be used to reduce server load. 
4) If you have a system where clients need to talk to each other frequently, you could use WebRTC to reduce the load on _your_ servers by having clients establish their own connections.
5) [Canva took this approach with presence/pointer sharing in their canvas editor](https://www.canva.dev/blog/engineering/realtime-mouse-pointers/) and it's a popular approach from collaborative document editing like [Google Docs](https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs) when used in conjunction with CRDTs which are better suited for a peer-to-peer architecture.

## Advantages
- Direct peer communication
- Lower latency
- Reduced server costs
- Native audio/video support

## Disadvantages
- Complex setup (> WebSockets)
- Requires signaling server
- NAT/firewall issues
- Connection setup delay

## Things to Discuss
1) If you're building a WebRTC app in a system design interview, it should be really obvious why you're using it.
2) Either you're trying to do video conferencing or the scale strictly requires you to have clients talk to each other directly.

> [!tip]
> Some interviews will get cute and introduce unrealistic constraints to try to get you to think outside the box, like "the system must run on Raspberry Pi". These _might_ be a case where a peer-to-peer architecture makes sense, but tread carefully.

3) Having some knowledge of the infra requirements (STUN/TURN, signaling servers, etc.) will give you the flexibility to make the best design decision for your system. 
4) You'll also want to speak pretty extensively about the communication patterns between peer clients and any eventual synchronization to a central server (almost all design questions will have some calling home to the mothership to store data or report results).

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[WebRTC]] — the protocol fundamentals (networking note)
- [[WebSockets - Full-Duplex Champion]] — the server-mediated alternative, and what the signaling server uses
- [[Choosing a Protocol]] — where WebRTC fits among the options
