---
tags: [system-design, hld, real-time, moc]
aliases: ["Realtime Updates"]
---

# Real-time Updates — Map of Content

> [!info] About this section
> How to deliver low-latency, push-based updates from servers to clients — the two "hops" (source → server, server → client) and the protocols and propagation strategies for each.

---

## Foundations
- [[The Two Hops]] — the problem, and the two-hop framing of the solution
- [[Networking 101]] — networking layers, request lifecycle, and L4 vs L7 load balancers

## First Hop — Client ↔ Server Protocols
- [[Simple Polling]] — the baseline: poll the server on an interval
- [[Long Polling]] — hold the request open until data is ready
- [[Server-Sent Events (SSE)]] — efficient one-way server → client streaming
- [[WebSockets - Full-Duplex Champion]] — bidirectional, high-frequency comms
- [[WebRTC - Peer-to-Peer]] — direct peer-to-peer for audio/video and data
- [[Choosing a Protocol]] — flowchart for picking the right first-hop tool

## Second Hop — Source → Server Propagation
- [[Server-Side Push and Pull]] — how updates get from the source to the server
- [[Pulling with Simple Polling]] — pull-based triggering via the database
- [[Pushing via Consistent Hashes]] — route users to owning servers with (consistent) hashing
- [[Pushing via Pub-Sub]] — broadcast updates through a pub/sub service

## Putting It Together
- [[When to Use]] — common real-time scenarios and when NOT to use real-time
- [[Common Deep Dives]] — reconnection, the celebrity problem, message ordering
- [[Conclusion]] — wrap-up and defaults

> [!tip] Interview mental model
> Start simple and escalate: polling → SSE → WebSockets → WebRTC. Solve the two hops separately, and reach for pub/sub on the server side unless connection state forces consistent hashing.
