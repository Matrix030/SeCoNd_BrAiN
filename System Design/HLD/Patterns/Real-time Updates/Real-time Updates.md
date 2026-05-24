
> **⚡ Real-time Updates** addresses the challenge of delivering immediate notifications and data changes from servers to clients as events occur. From chat applications where messages need instant delivery to live dashboards showing real-time metrics, users expect to be notified the moment something happens. This pattern covers the architectural approaches to enable low-latency, bidirectional communication.

## The Problem

Consider a collaborative document editor like Google Docs. 
1) When one user types a character, all other users viewing the document need to see that change within milliseconds.
2) In apps like this you can't have every user constantly polling the server for updates every few milliseconds without crushing your infrastructure.
3) The **core challenge** is establishing **efficient, persistent communication channels between clients and servers.** Standard HTTP follows a request-response model: clients ask for data, servers respond, then the connection closes.
4) This works great for traditional web browsing but breaks down when you need servers to proactively push updates to clients.

## The Solution

When systems require real-time updates, push notifications, etc, the solution requires two distinct pieces:

- The first "hop": how do we get updates from the server to the client?
- The second "hop": how do we get updates from the source to the server?

![[Pasted image 20260507170538.png]]

We'll break down each hop separately as they involve different trade-offs which work together.

### Client-Server Connection Protocols

1) The first "hop" is establishing efficient communication channels between clients and servers.
2) While traditional HTTP request-response works for a startling number of use-cases, real-time systems frequently need persistent connections or clever polling strategies to enable servers to **push** updates to clients. 

#### Networking 101
##### Networking Layers

1) In networks, each layer builds on the **abstractions** of the previous one.
2) This way, when you're requesting a webpage, you don't need to know which voltages represent a 1 or a 0 on the network wire - you just need to know how to use the next layer down the stack.
3) While the full networking stack is fascinating, there are three key layers that come up most often:
	- **Network Layer (Layer 3):** 
		- At this layer is IP, the protocol that handles routing and addressing.
		- It's responsible for breaking the data into packets, handling packet forwarding between networks, and providing best-effort delivery to any destination IP address on the network. 
		- However, there are no guarantees: packets can get lost, duplicated, or reordered along the way.
	- **Transport Layer (Layer 4):** 
		- At this layer, we have TCP and UDP, which provide end-to-end communication services:
	    - TCP is a **connection-oriented** protocol:
		    - before you can send data, you need to establish a connection with the other side. 
		    - Once the connection is established, it ensures that the data is delivered correctly and in order.
		    - This is a great guarantee to have but it also means that TCP connections take time to establish, resources to maintain, and bandwidth to use.
	    - UDP is a **connectionless** protocol: you can send data to any other IP address on the network without any prior setup. It does not ensure that the data is delivered correctly or in order. Spray and pray!
	- **Application Layer (Layer 7):** 
		- At the final layer are the application protocols like DNS, HTTP, Websockets, WebRTC. 
		- These are common protocols that build on top of TCP to provide a layer of abstraction for different types of data typically associated with web applications. 

These layers work together to enable all our network communications. 

##### Request Lifecycle

When you type a URL into your browser, several layers of networking protocols spring into action. Let's break down how these layers work together to retrieve a simple web page over HTTP.
- First, we use DNS to convert a human-readable domain name like hellointerview.com into an IP address like 32.42.52.62.
- Then, a series of carefully orchestrated steps begins:
![[Pasted image 20260516141450.png]]

1. **DNS Resolution**: The client starts by resolving the domain name of the website to an IP address using DNS (Domain Name System).
2. **TCP Handshake**: The client initiates a TCP connection with the server using a three-way handshake:
    - **SYN**: The client sends a SYN (synchronize) packet to the server to request a connection.
    - **SYN-ACK**: The server responds with a SYN-ACK (synchronize-acknowledge) packet to acknowledge the request.
    - **ACK**: The client sends an ACK (acknowledge) packet to establish the connection.
3. **HTTP Request**: Once the TCP connection is established, the client sends an HTTP GET request to the server to request the web page.
4. **Server Processing**: The server processes the request, retrieves the requested web page, and prepares an HTTP response.
5. **HTTP Response**: The server sends the HTTP response back to the client, which includes the requested web page content.
6. **TCP Teardown**: After the data transfer is complete, the client and server close the TCP connection using a four-way handshake:
    - **FIN**: The client sends a FIN (finish) packet to the server to terminate the connection.
    - **ACK**: The server acknowledges the FIN packet with an ACK.
    - **FIN**: The server sends a FIN packet to the client to terminate its side of the connection.
    - **ACK**: The client acknowledges the server's FIN packet with an ACK.

While the specific details of TCP handshakes might seem technical, two key points are particularly relevant for system design interviews:

1. First, each round trip between client and server adds **latency** to our request, including those to establish connections before we send our application data.
2. Second, the TCP connection itself represents **state** that both the client and server must maintain. Unless we use features like HTTP keep-alive, we need to repeat this connection setup process for every request - a potentially significant overhead.

Understanding when connections are established and how they are managed is crucial to touching on the important choices relevant for realtime updates.

> [!info]
> It's less common recently in BigTech, but it used to be a popular interview question to ask candidates to dive into the details of "what happens when you type (e.g.) hellointerview.com into your browser and press enter?".
> 
> Details like these aren't typically a part of a system design interview but it's helpful to understand the basics of networking. It may save you some headaches on the job!
