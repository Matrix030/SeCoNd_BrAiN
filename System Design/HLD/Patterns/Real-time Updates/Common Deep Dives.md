---
tags: [system-design, hld, real-time, fault-tolerance]
aliases: ["Real-time Deep Dives", "Common Deep Dives"]
---

# Common Deep Dives
1) Interviewers love to probe the operational challenges and edge cases of real-time systems.
2) Here are the most common follow-up questions you'll encounter.

## "How do you handle connection failures and reconnection?"
1) Real-world networks are unreliable. Mobile users lose connections constantly, WiFi drops out, and servers restart. Your real-time system needs graceful degradation and recovery.
2) The key challenge is detecting disconnections quickly and resuming without data loss.
3) WebSocket connections don't always signal when they break - a client might think it's connected while the server has already cleaned up the connection. 
4) Implementing heartbeat mechanisms helps detect these "zombie" connections.
5) For recovery, you need to track what messages or updates a client has received. 
6) When they reconnect, they should get everything they missed.
7) This often means maintaining a per-user message queue or implementing sequence numbers that clients can reference during reconnection. 
8) Using [Redis](https://www.hellointerview.com/learn/system-design/deep-dives/redis#redis-for-event-sourcing) streams for this is a popular option.

## "What happens when a single user has millions of followers who all need the same update?"
1) This is the classic "celebrity problem" in real-time systems.
2) When a celebrity posts, millions of users need that update simultaneously. Naive approaches create massive fan-out that can crash your system.
3) The solution involves strategic caching and hierarchical distribution. 
4) Instead of writing the update to millions of individual user feeds, cache the update once and distribute through multiple layers. 
5) Regional servers can pull the update and push to their local clients, reducing the load on any single component. 
6) More details on this in the [Batching and Hierarchical Aggregation](https://www.hellointerview.com/learn/system-design/patterns/scaling-writes#batching-and-hierarchical-aggregation) section of the Scaling Writes pattern.

![[Pasted image 20260525141828.png]]

## "How do you maintain message ordering across distributed servers?"
1) When multiple servers handle real-time updates, ensuring consistent ordering becomes complex.
2) Two messages sent milliseconds apart might arrive out of order if they travel different network paths or get processed by different servers.
3) Vector clocks or logical timestamps help establish ordering relationships between messages. 
4) Each server maintains its own clock, and messages include timestamp information that helps recipients determine the correct order.
5) For critical ordering requirements, you might need to funnel all related messages through a single server or partition. 
6) This trades some scalability for consistency guarantees, but simplifies the ordering problem significantly.

> [!tip]
> For most _product_-style system design, using a single server or partition is the way to go. There's a place for vector clocks and other techniques but they most often apply to deep infra rather than a question like "Design an Online Auction System". If all your messages make their way to a single host, stamping them with the correct timestamp and establishing a total order is straightforward.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[WebSockets - Full-Duplex Champion]] — the connections whose failures need handling
- [[Pushing via Pub-Sub]] — how updates fan out to many clients
- [[When to Use]] — the scenarios these questions come up in
