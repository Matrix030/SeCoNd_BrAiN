---
tags: [system-design, hld, real-time]
aliases: ["Choosing a Protocol", "First Hop Overview", "Real-time Protocol Flowchart"]
---

# Overview

1) There are a lot of options for delivering events from the server to the client.
2) Being familiar with the trade-offs associated with each will give you the flexibility to make the best design decision for your system. 
3) If you're in a hurry, the following flowchart will help you choose the right tool for the job.

- If you're not latency sensitive, **[[Simple Polling|simple polling]]** is a great baseline. 
	- You should probably start here unless you have a specific need in your system.
- If you don't need bi-directional communication, **[[Server-Sent Events (SSE)|SSE]]** is a great choice. 
	- It's lightweight and works well for many use cases.
	- There are some infrastructure considerations to keep in mind, but they're less invasive than with WebSocket and generally interviewers are less familiar with them or less critical if you don't address them.
- If you need frequent, bi-directional communication, **[[WebSockets - Full-Duplex Champion|WebSocket]]** is the way to go. It's more complex, but the performance benefits are huge.
- Finally, if you need to do audio/video calls, **[[WebRTC - Peer-to-Peer|WebRTC]]** is the only way to go. In some instances peer-to-peer collaboration can be enhanced with WebRTC, but you're unlikely to see it in a system design interview.
![[Pasted image 20260524192852.png]]

But now that we have the first hop out of the way, let's talk about how updates propagate from their source to the server in question.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Simple Polling]] — the latency-insensitive baseline
- [[Server-Sent Events (SSE)]] — the one-way default
- [[WebSockets - Full-Duplex Champion]] — for frequent bi-directional comms
- [[Server-Side Push and Pull]] — the second hop, covered next
