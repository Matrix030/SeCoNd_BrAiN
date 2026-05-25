---
tags: [system-design, hld, real-time]
aliases: ["When to Use Real-time Updates", "Real-time Scenarios"]
---

# When to Use
1) Real-time updates appear in almost every system design interview that involves user interaction or live data. 
2) Rather than waiting for the interviewer to ask about real-time features, proactively identify where immediate updates matter and address them in your initial design.
3) A strong candidate recognizes real-time requirements early.
4) When designing a chat application, immediately acknowledge that "messages need to be delivered instantly to all participants - I'll address that with WebSockets." 
5) For collaborative editing, mention that "character-level changes need sub-second propagation between users."

## Common Scenarios
1) **[Chat Applications](https://www.hellointerview.com/learn/system-design/problem-breakdowns/whatsapp)**: 
	- The classic real-time use case. 
	- Messages must appear instantly across all participants. 
	- WebSockets handle the bidirectional communication perfectly, while pub/sub distributes messages to the right servers.
	- Consider message ordering, typing indicators, and presence status.

2) **[Live Comments](https://www.hellointerview.com/learn/system-design/problem-breakdowns/fb-live-comments)**: 
	- High-volume, real-time social interaction during live events. 
	- Millions of viewers commenting simultaneously creates extreme fan-out problems.
	- Hierarchical aggregation and careful batching prevent system overload while maintaining the live feel.

3) **[Collaborative Document Editing](https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs)**: 
	- Character-level changes need instant propagation between users. 
	- WebSockets provide the low-latency communication, while operational transforms or CRDTs handle conflict resolution. 
	- User cursors and selection highlighting add additional real-time complexity.

> [!tip]
> Collaborative editing commonly requires us to not only deal with getting updates to clients with low latency and high frequency, but also to deal with conflicts and ensure that the state of the document is consistent. If users can be typing on top of one another, they often need to be able to deal with the conflicts that arise. We talk about two approaches for dealing with this in the [Google Docs](https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs) breakdown: CRDTs and Operational Transforms.

3) **[Live Dashboards and Analytics](https://www.hellointerview.com/learn/system-design/problem-breakdowns/uber)**: 
	- Business metrics and operational data that changes constantly. 
	- Server-Sent Events work well for one-way data flow from servers to dashboards.
	- Consider data aggregation intervals and what constitutes "real-time enough" for business decisions.

4) **Gaming and Interactive Applications**:
	- Multiplayer games need the lowest latency possible.
	- WebRTC enables peer-to-peer communication for reduced latency, while WebSockets handle server coordination. 
	- Consider different update frequencies for different game elements.

## When NOT to Use
1) Avoid real-time updates when you can get away with a simple polling model.
2) If you're not latency sensitive, polling is a great baseline and minimizes complexity — a property highly valued in senior+ interviews. 
3) By polling you avoid both hops: you don't need to worry about the client->server protocols AND you don't have to handle propagation from the event source.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Simple Polling]] — the baseline to reach for when real-time isn't required
- [[WebSockets - Full-Duplex Champion]] — chat and collaborative editing
- [[Common Deep Dives]] — the follow-up questions these scenarios trigger
