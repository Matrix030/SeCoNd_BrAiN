---
tags: [system-design, hld, networking, real-time, websockets]
aliases: ["WebSocket", "WS"]
---

# WebSockets: Real-Time Bidirectional Communication

1) WebSockets provide a persistent, TCP-style connection between client and server, allowing for real-time, bidirectional communication with broad support (including browsers).
2) Unlike HTTP's request-response model, WebSockets enable servers to **push** data to clients without being prompted by a new request.
3) Similarly clients can push data back to the server without the same wait.
4) WebSockets are initiated via an HTTP "upgrade" protocol, which allows an existing TCP connection to change L7 protocols.
5) This is super convenient because it means you can utilize some of the existing HTTP session information (e.g. cookies, headers, etc.) to your advantage.

> [!warning]
> Just because clients can upgrade from HTTP to WebSocket doesn't mean that the infrastructure will support it. Every piece of infrastructure between the client and server will need to support WebSocket connections.
>
> If you've ever implemented Websockets you've probably hit a bunch of issues with firewalls, proxies, load balancers, and other infrastructure that don't support WebSocket connections.

##### How it Works

1. Client initiates WebSocket handshake over HTTP (with a backing TCP connection)
2. Connection upgrades to WebSocket protocol, WebSocket takes over the TCP connection
3. Both client and server can send binary messages to each other over the connection
4. The connection stays open until explicitly closed

- WebSockets don't dictate an application protocol, you effectively have a channel where you can send binary packets to the server from the client and vice versa.
- This means you'll need some way of defining what it is your client and server are exchanging.
- For many WebSocket applications, simple serialized JSON messages are a great option! This also gives you a chance to define the API of your service for your design:

![[Pasted image 20260418210131.png]]

##### Where to Use It

1) WebSockets come up in system design interviews when you need **high-frequency**, **persistent**, **bi-directional** communication between client and server. Think real-time applications, games, and other use-cases where you need to send and receive messages as soon as they happen.
2) For applications where either you just need to be able to send requests and receive responses, or situations where you can make due with the push notifications provided by SSE, WebSockets are overkill.

> [!warning]
> WebSockets are powerful, but the infra required to support them can be expensive and the overhead of stateful connections (especially at scale) will require significant accommodations in your design. Hold off unless you really need them!

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[TCP]] — WebSockets run over a persistent TCP connection
- [[Server-Sent Events]] — simpler alternative when only server→client push is needed
- [[Load Balancing]] — use L4 load balancers for WebSocket connections
- [[WebRTC]] — peer-to-peer alternative for audio/video
