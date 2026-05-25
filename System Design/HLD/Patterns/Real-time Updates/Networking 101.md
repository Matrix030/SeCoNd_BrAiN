---
tags: [system-design, hld, real-time, networking, load-balancing]
aliases: ["Networking 101", "Real-time Networking Primer"]
---

# Networking 101

1) The first "hop" is establishing efficient communication channels between clients and servers.
2) While traditional HTTP request-response works for a startling number of use-cases, real-time systems frequently need persistent connections or clever polling strategies to enable servers to **push** updates to clients. 

## Networking Layers

1) In networks, each layer builds on the **abstractions** of the previous one.
2) This way, when you're requesting a webpage, you don't need to know which voltages represent a 1 or a 0 on the network wire - you just need to know how to use the next layer down the stack.
3) While the full networking stack is fascinating, there are three key layers that come up most often:
	- **Network Layer (Layer 3):** 
		- At this layer is IP, the protocol that handles routing and addressing.
		- It's responsible for breaking the data into packets, handling packet forwarding between networks, and providing best-effort delivery to any destination IP address on the network. 
		- However, there are no guarantees: packets can get lost, duplicated, or reordered along the way.
	- **Transport Layer (Layer 4):** 
		- At this layer, we have [[TCP]] and [[UDP]], which provide end-to-end communication services:
	    - TCP is a **connection-oriented** protocol:
		    - before you can send data, you need to establish a connection with the other side. 
		    - Once the connection is established, it ensures that the data is delivered correctly and in order.
		    - This is a great guarantee to have but it also means that TCP connections take time to establish, resources to maintain, and bandwidth to use.
	    - UDP is a **connectionless** protocol: you can send data to any other IP address on the network without any prior setup. It does not ensure that the data is delivered correctly or in order. Spray and pray!
	- **Application Layer (Layer 7):** 
		- At the final layer are the application protocols like DNS, [[HTTP and HTTPS|HTTP]], [[WebSockets|Websockets]], [[WebRTC]]. 
		- These are common protocols that build on top of TCP to provide a layer of abstraction for different types of data typically associated with web applications. 

These layers work together to enable all our network communications. 

## Request Lifecycle

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

## With Load Balancers

1) In real-world systems, we typically have multiple servers working together behind a [[Load Balancing|load balancer]]. 
2) Load balancers distribute incoming requests across these servers to ensure even load distribution and high availability. 
3) There are two main types of load balancers you'll encounter in system design interviews: Layer 4 and Layer 7.
### Layer 4 Load Balancers

1) Layer 4 load balancers operate at the transport layer (TCP/UDP). 
2) They make routing decisions based on network information like IP addresses and ports, without looking at the actual content of the packets.
3) The effect of a L4 load balancer is as-if you randomly selected a backend server and assumed that TCP connections were established directly between the client and that server — this mental model is not far off.
![[Pasted image 20260524124831.png]]

4) Key characteristics of L4 load balancers:
	
	- Maintain persistent TCP connections between client and server.
	- Fast and efficient due to minimal packet inspection.
	- Cannot make routing decisions based on application data.
	- Typically used when raw performance is the priority.
5) For example, if a client establishes a TCP connection through an L4 load balancer, that same server will handle all subsequent requests within that TCP session.
6) This makes L4 load balancers particularly well-suited for protocols that require persistent connections, like WebSocket connections.
7) At a conceptual level, _it's as if we have a direct TCP connection between client and server which we can use to communicate at higher layers_.

### Layer 7 Load Balancers
1) Layer 7 load balancers operate at the application layer, understanding protocols like HTTP.
2) They can examine the actual content of each request and make more intelligent routing decisions.
![[Pasted image 20260524125357.png]]

3) Key characteristics of L7 load balancers:
	- Terminate incoming connections and create new ones to backend servers.
	- Can route based on request content (URL, headers, cookies, etc.).
	- More CPU-intensive due to packet inspection.
	- Provide more flexibility and features.
	- Better suited for HTTP-based traffic.
4) For example, an L7 load balancer could route all API requests to one set of servers while sending web page requests to another (providing similar functionality to an [API Gateway](https://www.hellointerview.com/learn/system-design/deep-dives/api-gateway)), or it could ensure that all requests from a specific user go to the same server based on a cookie.
5) The underlying TCP connection that's made to your server via an L7 load balancer is not all that relevant! It's just a way for the load balancer to forward L7 requests, like HTTP, to your server.

> [!info]
> The choice between L4 and L7 load balancers often comes up in system design interviews when discussing real-time features. There are some L7 load balancers which explicitly support connection-oriented protocols like WebSockets, but generally speaking L4 load balancers are better for WebSocket connections, while L7 load balancers offer more flexibility for HTTP-based solutions like long polling. We'll get into more detail on this in the next section.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[A Simple Web Request]] — the full DNS → TCP → HTTP request lifecycle
- [[Load Balancing]] — L4 vs L7 load balancers in depth
- [[Networking Layers]] — the OSI layers reference note
- [[The Two Hops]] — why the first hop needs persistent connections
