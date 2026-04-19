---
tags: [system-design, hld, networking, tcp, http]
aliases: ["Web Request", "Simple Web Request"]
---

# A Simple Web Request

When you type a URL into your browser, several layers of networking protocols spring into action. Let's break down how these layers work together to retrieve a simple web page over HTTP on TCP.

1) First, we use [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) to convert a human-readable domain name like hellointerview.com into an IP address like 32.42.52.62.
2) Then, a series of carefully orchestrated steps begins. We set up a TCP connection over IP, send our HTTP request, get a response, and tear down the connection.

![[Pasted image 20260418115248.png]]

1) **DNS Resolution**: The client starts by resolving the domain name of the website to an IP address using DNS (Domain Name System).
2) **TCP Handshake**: The client initiates a TCP connection with the server using a three-way handshake:
    - **SYN**: The client sends a SYN (synchronize) packet to the server to request a connection.
    - **SYN-ACK**: The server responds with a SYN-ACK (synchronize-acknowledge) packet to acknowledge the request.
    - **ACK**: The client sends an ACK (acknowledge) packet to establish the connection.
3) **HTTP Request**: Once the TCP connection is established, the client sends an HTTP GET request to the server to request the web page.
4) **Server Processing**: The server processes the request, retrieves the requested web page, and prepares an HTTP response. (This is usually the only latency most SWE's think about and control)
5) **HTTP Response**: The server sends the HTTP response back to the client, which includes the requested web page content.
6) **TCP Teardown**: After the data transfer is complete, the client and server close the TCP connection using a four-way handshake:
    - **FIN**: The client sends a FIN (finish) packet to the server to terminate the connection.
    - **ACK**: The server acknowledges the FIN packet with an ACK.
    - **FIN**: The server sends a FIN packet to the client to terminate its side of the connection.
    - **ACK**: The client acknowledges the server's FIN packet with an ACK.


there's a few things to observe:
1) First, as an application developer we are able to simplify our mental models dramatically.
	- The application can take for granted that the data is transmitted with a degree of reliability and ordering: the TCP layer ensures that the data is delivered correctly and in order, and will provide a response to the application if it doesn't arrive.
	- We also never have to concern ourselves with finding a specific server in the world and driving a pulse train of electrons to get there.
	- With DNS, we can look up the IP address, and with IP the various networking hardware between us, our ISP, backbone providers, etc. can route the data to the destination. Nice!

2) Second, while we have one conceptual "request" and "response" here, there were many more packets and requests exchanged between servers to make it happen.
	- All of these introduce latency that we can ignore, until we can't.
	- The higher in the stack we go, the more latency and processing required.

3) Finally note that the connection between the client and server is a **state** that both the client and server must maintain.
	- Unless we use features like HTTP keep-alive or HTTP/2 multiplexing, we need to repeat this connection setup process for every request - a potentially significant overhead.
	- This will become important for designing systems which need persistent connections, like those handling [Realtime Updates](https://www.hellointerview.com/learn/system-design/deep-dives/realtime-updates).

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[Networking Layers]] — the OSI layers at play here
- [[TCP]] — the transport layer protocol driving this example
- [[HTTP and HTTPS]] — the application layer protocol
- [[WebSockets]] — persistent connections that avoid repeated handshakes
