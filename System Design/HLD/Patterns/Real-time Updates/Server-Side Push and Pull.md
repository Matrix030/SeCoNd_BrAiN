---
tags: [system-design, hld, real-time]
aliases: ["Server-Side Push/pull", "Second Hop", "Server-Side Push and Pull"]
---

# Server-Side Push/pull
1) Now that we've established our options for the hop from server to client (Simple Polling, Long-Polling, SSE, WebSockets, WebRTC), let's talk about how we can propagate updates from the source to the server.
![[Pasted image 20260524193432.png]]

2) Invariably our system is somehow _producing_ updates that we want to propagate to our clients. 
3) This could be other users making edits to a shared documents, drivers making location updates, or friends sending messages to a shared chat.
4) Making sure these updates get to their ultimate destination is closely tied to how we propagate updates from the source to the server. Said differently, we need a **trigger**.

When it comes to triggering, there's three patterns that you'll commonly see:
1. Pulling via Polling
2. Pushing via Consistent Hashes
3. Pushing via Pub/Sub

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Pulling with Simple Polling]] — pull-based triggering via the database
- [[Pushing via Consistent Hashes]] — route users to owning servers with hashing
- [[Pushing via Pub-Sub]] — broadcast updates through a pub/sub service
- [[Choosing a Protocol]] — the first hop these triggers feed into
