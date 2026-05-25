---
tags: [system-design, hld, real-time]
aliases: ["Pushing via Pub/Sub", "Pub/Sub for Updates"]
---

# Pushing via Pub/Sub
1) Another approach to triggering updates is to use a **pub/sub** model.
2) In this model, we have a single service that is responsible for collecting updates from the source and then broadcasting them to all interested clients. Popular options here include Kafka and Redis.
3) The pub/sub service becomes the biggest source of _state_ for our realtime updates. 
4) Our persistent connections are now made to lightweight servers which simply subscribe to the relevant topics and forward the updates to the appropriate clients. 
5) Refer to these servers as _endpoint_ servers.
6) When clients connect, we don't need them to connect to a specific endpoint server (like we did with consistent hashing) and instead can connect to any of them. Once connected, the endpoint server will register the client with the pub/sub server so that any updates can be sent to them.
![[Pasted image 20260525141157.png]]

On the connection side, the following happens:

1. The client establishes a connection with an endpoint server.
2. The endpoint server registers the client with the Pub/Sub service, often by creating a topic, subscribing to it, and keeping a mapping from topics to the connections associated with them.
![[Pasted image 20260525141210.png]]

On the update broadcasting side, the following happens:
1. Updates are pushed to the Pub/Sub service to the relevant topic.
2. The Pub/Sub service broadcasts the update to all clients subscribed to that topic.
3. The endpoint server receives the update, looks up which client is subscribed to that topic, and forwards the update to that client over the existing connection.

For our chat application, we'll create a topic for each user. When the client connects to our endpoint, it will subscribe to the topic associated with the connected user. When we need to send messages, we publish them to that user's topic and the Pub/Sub service will broadcast them to all of the subscribed servers - then these servers will forward them to the appropriate clients over the existing connections.

## Advantages
- Managing load on endpoint servers is easy, we can use a simple load balancer with "least connections" strategy.
- We can broadcast updates to a large number of clients efficiently.
- We minimize the proliferation of state through our system.

## Disadvantages
- We don't know whether subscribers are connected to the endpoint server, or when they disconnect.
- The Pub/Sub service becomes a single point of failure and bottleneck.
- We introduce an additional layer of indirection which can add to latency.
- There exist many-to-many connections between Pub/Sub service hosts and the endpoint servers.

## When to Use Pub/Sub
1) Pub/Sub is a great choice when you need to broadcast updates to a large number of clients. 
2) It's easy to set up and requires little overhead on the part of the endpoint servers. 
3) The latency impact is minimal (<10ms). 
4) If you don't need to respond to connect/disconnect events or maintain a lot of state associated with a given client, this is a great approach.
## Things to Discuss in Your Interview
1) If you're using a pub/sub model, you'll probably need to talk about the single point of failure and bottleneck of the pub/sub service.
2) Redis cluster is a popular way to scale pub/sub service which involves sharding the subscriptions by their key across multiple hosts. 
3) This scales up the number of subscriptions you can support and the throughput.
4) Introducing a cluster for the Pub/Sub component means you'll manage the many-to-many connections between the pub/sub service and the endpoint servers (each endpoint server will be connected to all hosts in the cluster). 
5) In some cases this can be managed by carefully choosing topics to partition the service, but in many cases the number of nodes in the cluster is small.
6) For inbound connections to the endpoint servers, you'll probably want to use a load balancer with a "least connections" strategy.
7) This will help ensure that you're distributing the load across the servers in the cluster. 
8) Since the connection itself (and the messages sent across it) are effectively the only resource being consumed, load balancing based on connections is a great way to manage the load.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Pushing via Consistent Hashes]] — the alternative when connections carry heavy state
- [[Pulling with Simple Polling]] — the pull-based trigger
- [[Server-Side Push and Pull]] — the three server-side trigger patterns
