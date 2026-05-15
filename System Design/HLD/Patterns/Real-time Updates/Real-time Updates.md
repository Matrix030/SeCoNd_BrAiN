
> **⚡ Real-time Updates** addresses the challenge of delivering immediate notifications and data changes from servers to clients as events occur. From chat applications where messages need instant delivery to live dashboards showing real-time metrics, users expect to be notified the moment something happens. This pattern covers the architectural approaches to enable low-latency, bidirectional communication.

## The Problem

Consider a collaborative document editor like Google Docs. 
1) When one user types a character, all other users viewing the document need to see that change within milliseconds.
2) In apps like this you can't have every user constantly polling the server for updates every few milliseconds without crushing your infrastructure.
3) The **core challenge** is establishing **efficient, persistent communication channels between clients and servers.** Standard HTTP follows a request-response model: clients ask for data, servers respond, then the connection closes.
4) This works great for traditional web browsing but breaks down when you need servers to proactively push updates to clients.

## The Solution

When systems require real-time updates, push notifications, etc, the solution requires two distinct pieces:

1. The first "hop": how do we get updates from the server to the client?
2. The second "hop": how do we get updates from the source to the server?
![[Pasted image 20260507170538.png]]

We'll break down each hop separately as they involve different trade-offs which work together.

### Client-Server Connection Protocols

1) The first "hop" is establishing efficient communication channels between clients and servers.
2) While traditional HTTP request-response works for a startling number of use-cases, real-time systems frequently need persistent connections or clever polling strategies to enable servers to **push** updates to clients. This is where we get into the nitty-gritty of networking.

#### Networking 101

1) Before diving into the different protocols for facilitating real-time updates, it's helpful to understand a bit about how networking works — in some sense the problems we're talking about here are just networking problems! 
2) Networks are built on a layered architecture (the so-called ["OSI model"](https://en.wikipedia.org/wiki/OSI_model)) which greatly simplifies the world for us application developers who sit on top of it.
##### Networking Layers

1) In networks, each layer builds on the **abstractions** of the previous one.
2) This way, when you're requesting a webpage, you don't need to know which voltages represent a 1 or a 0 on the network wire - you just need to know how to use the next layer down the stack.
3) While the full networking stack is fascinating, there are three key layers that come up most often in system design interviews:

- **Network Layer (Layer 3):** 
	- At this layer is IP, the protocol that handles routing and addressing.
	- It's responsible for breaking the data into packets, handling packet forwarding between networks, and providing best-effort delivery to any destination IP address on the network. 
	- However, there are no guarantees: packets can get lost, duplicated, or reordered along the way.
- **Transport Layer (Layer 4):** At this layer, we have TCP and UDP, which provide end-to-end communication services:
    - TCP is a **connection-oriented** protocol: before you can send data, you need to establish a connection with the other side. Once the connection is established, it ensures that the data is delivered correctly and in order. This is a great guarantee to have but it also means that TCP connections take time to establish, resources to maintain, and bandwidth to use.
    - UDP is a **connectionless** protocol: you can send data to any other IP address on the network without any prior setup. It does not ensure that the data is delivered correctly or in order. Spray and pray!
- **Application Layer (Layer 7):** At the final layer are the application protocols like DNS, HTTP, Websockets, WebRTC. These are common protocols that build on top of TCP to provide a layer of abstraction for different types of data typically associated with web applications. We'll get into them in a bit!

These layers work together to enable all our network communications. To see how they interact in practice, let's walk through a concrete example of how a simple web request works.

##### Request Lifecycle

When you type a URL into your browser, several layers of networking protocols spring into action. Let's break down how these layers work together to retrieve a simple web page over HTTP. First, we use DNS to convert a human-readable domain name like hellointerview.com into an IP address like 32.42.52.62. Then, a series of carefully orchestrated steps begins: