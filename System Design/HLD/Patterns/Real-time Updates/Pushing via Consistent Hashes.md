---
tags: [system-design, hld, real-time, scaling]
aliases: ["Pushing via Consistent Hashes", "Consistent Hashing for Connections"]
---

# Pushing via Consistent Hashes
1) The remaining approaches involve **pushing** updates to the clients.
2) In many of the client update mechanisms that we discussed above (long-polling, SSE, WebSockets) the client has a persistent connection to one server and that server is responsible for sending updates to the client.

But this creates a problem! For our chat application, in order to send a message to User C, we need to know which server they are connected to.
![[Pasted image 20260524194644.png]]

Ideally, when a message needs to be sent, I would:
1. Figure out which server User C is connected to.
2. Send the message to that server.
3. That server will look up which (websocket, SSE, long-polling) request is associated with User C.
4. The server will then write the message via the appropriate connection.

There are two common ways to handle this situation, and the first is to use **hashing**. Let's build up our intuition for this in two steps.
## Simple Hashing
1) Our first approach might be to use a simple modulo operation to figure out which server is responsible for a given user.
2) Then, we'll always have 1 server who "owns" the connections for that user.
3) To do this, we'll have a central service that knows how many servers there are N and can assign them each a number 0 through N-1.
4) This is frequently Apache [ZooKeeper](https://www.hellointerview.com/learn/system-design/deep-dives/zookeeper) or Etcd which allows us to manage this metadata and allows the servers to keep in sync as it updates, though in practice there are many alternatives.
5) We'll make the server number n responsible for user u % N. 
6) When clients initially connect to our service, we can either:
	- Directly connect them to the appropriate server (e.g. by having them know the hash, N, and the server addressess associated with each index). 
	- Have them randomly connect to any of the servers and have that server redirect them to the appropriate server based on internal data.

![[Pasted image 20260525135139.png]]

When a client connects, the following happens:
1. The client connects to a random server.
2. The server uses Zookeeper's server list to compute which server is responsible for the client (by hashing their ID and applying modulo N).
3. The server redirects the client to the appropriate server.
4. The client connects to the correct server.
5. The server adds that client to a map of connections.

Now we're ready to send updates and messages!
When we want to send messages to User C, we can simply hash the user's id to figure out which server is responsible for them and send the message there.

![[Pasted image 20260525140259.png]]

1. Our Update Server stays connected to Zookeeper and knows the addresses of all servers and the modulo N.
2. When the Update Server needs to send a message to User C, it can hash the user's id to figure out which server is responsible for them (Server 2) and sends the message there.
3. Server 2 receives the message, looks up which connection is associated with User C, and sends the message to that connection.

- This approach works because we always know that a single server is responsible for a given user (or entity, or ID, or whatever). 
- All inbound connections go to that server and, if we want to use the connection associated with that entity, we know to pass it to that server for forwarding to the end client.

## Consistent Hashing
1) The hashing approach works great when N is fixed, but becomes problematic when we need to scale our service up or down.
2) With simple modulo hashing, changing the number of servers would require almost all users to disconnect and reconnect to different servers - an expensive operation that disrupts service.
3) [Consistent hashing](https://www.hellointerview.com/learn/system-design/deep-dives/consistent-hashing) solves this by minimizing the number of connections that need to move when scaling. 
4) It maps both servers and users onto a hash ring, and each user connects to the next server they encounter when moving clockwise around the ring.
![[Pasted image 20260525140400.png]]

When we add or remove servers, only the users in the affected segments of the ring need to move. This greatly reduces connection churn during scaling operations.

![[Pasted image 20260525140415.png]]

## Advantages
- Predictable server assignment
- Minimal connection disruption during scaling
- Works well with stateful connections
- Easy to add/remove servers

## Disadvantages
- Complex to implement correctly
- Requires coordination service (like Zookeeper)
- All servers need to maintain routing information
- Connection state is lost if a server fails

## When to Use Consistent Hashing
1) Consistent hashing is ideal when you need to maintain persistent connections (WebSocket/SSE) and your system needs to scale dynamically. 
2) It's particularly valuable when each connection requires significant server-side state that would be expensive to rebuild.
3) Because consistent hashing assigns each user to a predictable server, that state only lives in one place.
4) And when you scale up or down, only a small fraction of connections need to move, so you're not rebuilding expensive state for every user at once.
5) For example, in [the Google Docs design](https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs), connections are associated with specific documents that require substantial state to maintain collaborative editing functionality. 
6) Loading a document, applying all pending operations, and syncing with collaborators is expensive. Consistent hashing keeps that state on a single server while allowing for scaling.

However, if you're just passing small messages to clients without much associated state, you're probably better off using the next approach: Pub/Sub. With pub/sub, the state lives in the pub/sub service itself, so your endpoint servers stay lightweight and interchangeable.

## Things to Discuss 
1) If you introduce a consistent hashing approach in an interview, you'll want to be able to discuss not only how the updates are routed (e.g. a coordination service like Zookeeper or etcd).
2) Interviewers are usually interested to understand how the system scales: what happens when we need to increase or decrease the number of nodes. 
3) For those instances, you'll want to be able to share your knowledge about consistent hashing but also talk about the orchestration logic necessary to make it work. In practice, this usually means:
	1. Signaling the beginning of a scaling event. Recording both the old and new server assignments.
	2. Slowly disconnecting clients from the old server and having them reconnect to their newly assigned server.
	3. Signaling the end of the scaling event and updating the coordination service with the new server assignments.
	4. In the interim, having messages which are sent to both the old and new server until they're fully transitioned.

4) The mechanics of discovering the initial server assignments is also interesting. 
5) Having clients know about the internal structure of your system can be problematic, but there are performance tradeoffs associated with redirecting clients to the correct server or requiring them to do a round-trip to a central server to look up the correct one.
6) Especially during scaling events, any central registration service may become a bottleneck so it's important to discuss the tradeoffs with your interviewer.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Pushing via Pub-Sub]] — the lighter-weight alternative when connections are stateless
- [[WebSockets - Full-Duplex Champion]] — the stateful connections this routes
- [[Server-Side Push and Pull]] — the three server-side trigger patterns
