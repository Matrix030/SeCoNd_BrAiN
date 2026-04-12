Core concepts are the fundamental principles and techniques that form the foundation of every system design interview.

## Networking Essentials

The most important decision you'll make is choosing your communication protocol. For most systems, you'll default to HTTP over TCP.

WebSockets and Server-Sent Events (SSE) come up when you need real-time updates.

- The key difference: **SSE is unidirectional** - the client makes an initial HTTP request to open the connection, and then the server pushes data down that connection (like live scores or notifications).
- The client can't send additional data over the same SSE connection.
- **WebSockets handle true bidirectional communication** where both sides send messages freely (like chat or live collaboration). 
- SSE is simpler to implement and works better with standard HTTP infrastructure.
- WebSockets are necessary when clients need to push data back to the server frequently.

Both are **stateful connections** - you can't just throw them behind a standard load balancer. You'll need to think about connection persistence and what happens when a server goes down with thousands of active connections.

**gRPC** is worth mentioning for internal service-to-service communication when performance is critical. It uses binary serialization and HTTP/2, making it significantly faster than JSON over HTTP. But you won't use it for public-facing APIs because browsers don't natively support gRPC (gRPC-Web exists as a workaround, but it requires a proxy and doesn't support all gRPC features). A common pattern is REST for external APIs and gRPC internally.

**Load balancing** is another area.
- **Layer 7 load balancers** operate at the application level and can route based on the actual HTTP request content. You can send API calls to one service and web page requests to another.
- **Layer 4 load balancers** work at the TCP level and are faster but dumber. They just distribute connections without looking at the content. For WebSockets, you typically need Layer 4 balancing because you're maintaining a persistent TCP connection.

[!warning]
A common mistake is proposing WebSockets when HTTP with long polling or Server-Sent Events would work fine. WebSockets add significant complexity for maintaining stateful connections at scale. Only reach for them when you genuinely need bidirectional real-time communication, not just because "real-time" is mentioned in the problem.

## [API Design]

[!tip]
You should be able to sketch out 4-5 key endpoints in a couple minutes and move on. If you find yourself still designing API details 10 minutes into the interview, you're going too deep.

There are a few concepts worth mentioning when they come up. If you're returning large result sets, you'll need **pagination**. **Cursor-based** works better for real-time data where new items get added frequently, but **offset-based** is fine for most cases. For authentication, use **JWT tokens** for user sessions and API keys for service-to-service calls. And if your system could get hammered by bots or abuse, mention **rate limiting**. But don't go deep on any of these unless the interviewer specifically asks.

## [Data Modeling]

The decisions you make about what data to store and how to structure it directly affect performance, scalability, and how painful it is to build and maintain your system.

![[Pasted image 20260412183349.png]]

The first big choice is **relational versus NoSQL**. 
- **Relational databases** like Postgres work great when you have structured data with clear relationships and need strong consistency.
- Things like user accounts linking to orders linking to products.
- You can express complex queries with SQL, use transactions to keep data consistent, and enforce foreign key constraints.
- **NoSQL** databases like DynamoDB or MongoDB shine when you need flexible schemas (your data structure changes frequently) or you need to scale horizontally across many servers without complex joins.

















