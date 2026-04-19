---
tags: [system-design, hld, networking, load-balancing, scaling]
aliases: ["Load Balancer", "LB", "L4 Load Balancer", "L7 Load Balancer"]
---

# Load Balancing

How do we **scale** our designs?

For scaling, we have two options:
- bigger servers (vertical scaling)
- more servers (horizontal scaling).

![[Pasted image 20260419102157.png]]

Modern hardware is incredibly powerful and the days of requiring thousands of tiny servers when a few larger ones can handle the load are over.

That said, the reality is that **the most common pattern for scaling you'll see is horizontal scaling**: we're going to add more servers to handle the load. But just adding boxes to our whiteboard won't help if we don't tell our clients which server to talk to.

Enter: Load Balancing.
![[Pasted image 20260419102752.png|646]]

### Types of Load Balancing

1) We need to spread the incoming requests (load) by deciding which server should handle each request.
2) There's two ways to handle load balancing: on the client side or on the server side. Both have their pros and cons.

#### Client-Side Load Balancing

1) With client-side load balancing, the client itself decides which server to talk to.
2) Usually this involves the client making a request to a service registry or directory which contains the list of available servers.
3) Then the client makes a request to one of those servers directly.
4) The client will need to periodically poll or be pushed updates when things change.
5) Client-side load balancing can be very fast and efficient.
6) Since the client is making the decision, it can choose the fastest server without any additional latency.
7) Instead of using a full network hop to get routed to the right server on every request, we only need to (periodically) sync our list of servers with the server registry.

##### Example: Redis Cluster

- A great example of this is Redis Cluster, Redis cluster nodes maintain a **gossip protocol** between each other to share information about the cluster: which nodes are present, their status, etc. Every node knows about every other node!
- In order to connect to a Redis Cluster, the client will make a request to any of the nodes in the cluster and ask about both the nodes participating in the cluster and the shards of data they contain.
- When it comes time to read or write data, the client hashes the key to determine which shard to send the request to, then uses the locally retrieved node information to decide which node to talk to.
- If you send a request to the wrong node, Redis will helpfully send you a MOVED response to let you know you got the wrong node.

##### Example: DNS

- Another example of "client-side" load balancing is DNS.
- When you make a request to a domain name like example.com, your DNS resolver will return a rotated list of IP addresses for the domain.
- Each new request will get a different ordering of IP addresses (or even a different set entirely).
- Because each client gets a different ordering of IP addresses, they're also going to hit different servers.

> [!info]
> This behavior of DNS is also how you avoid a single point of failure with a load balancer! You set up two load balancers (in different data centers or regions, to be safe) and use DNS to rotate between them. If one goes down, clients will automatically start trying the other one.

##### Where to Use It

- Client-side load balancing can work great in two different scenarios: either
	1) we have a small number of clients that we control, (e.g. the Redis Cluster client, or gRPC's client-side load balancing for internal services)
	2) we have a large number of clients but we can tolerate slow updates (e.g. DNS).
- If we have a small number of clients that we control, getting them updates when we add or remove servers is easy!
- In the case of a large number of clients, the reason we care about the latency of updates is because the amount of time it takes will scale with the number of clients we have to notify.
- In DNS' case, entries have a TTL (time to live) which is the amount of time the entry is valid for.
- This allows far-flung DNS servers to cache entries for _their own_ clients, but means that our updates cannot be faster than the TTL.

![[Pasted image 20260419104838.png]]

client-side load balancing works remarkably well for internal microservices (it's actually built in to gRPC).

#### Dedicated Load Balancers

1) We may not want our clients to have to refresh their list of servers or even know about the existence of multiple servers on the backend.
2) We might have a large number of clients that we don't control but need to retrieve updates quickly.
3) In these cases, we'll use a dedicated load balancer:
	- a server or hardware device that sits between the client and the backend servers and makes decisions about which server to send the request to.

![[Pasted image 20260419122324.png]]

These load balancers can operate at different layers of the protocol stack and which you choose will depend, in part, on what your application needs.

Having a dedicated load balancer implies an additional hop in each request:
- first to the load balancer, then to the server which needs to serve the request.
- But in exchange we get very fast updates to our list of servers and fine-grained control over how we route requests.

##### Layer 4 Load Balancers

1) Layer 4 load balancers operate at the transport layer (TCP/UDP).
2) They make routing decisions based on network information like IP addresses and ports, **without looking at the actual content of the packets**.
3) The effect of a L4 load balancer is as-if you randomly selected a backend server and assumed that TCP connections were established directly between the client and that server.
![[Pasted image 20260419123156.png]]

Layer 4 load balancers have some key characteristics, they:
- Maintain persistent TCP connections between client and server.
- Are fast and efficient due to minimal packet inspection.
- Cannot make routing decisions based on application data.
- Are typically used when raw performance is the priority.

For example, if a client establishes a TCP connection through an L4 load balancer, that same server will handle all subsequent requests within that TCP session. This makes L4 load balancers particularly well-suited for protocols that require persistent connections, like WebSocket connections. At a conceptual level, _it's as if we have a direct TCP connection between client and server which we can use to communicate at higher layers_.

###### Where to Use It

L4 load balancers are great for WebSocket connections and other protocols that **require persistent connections**. They're also great for high-performance applications that don't require much application-level processing.

If you're using websockets in your interview, you probably want to use an L4 load balancer. For everything else, a Layer 7 load balancer is probably a better fit.

##### Layer 7 Load Balancers

1) Layer 7 load balancers operate at the application layer, understanding protocols like HTTP.
2) They can **examine the actual content of each request and make more intelligent routing decisions**.
3) Unlike Layer 4 load balancers, the connection-level details are not that relevant.
4) Layer 7 load balancers receive an application-layer request (like an HTTP GET) and forward _that request_ to the appropriate backend server.

![[Pasted image 20260419123620.png]]

Layer 7 load balancers have some key characteristics, they ...

- Terminate incoming connections and create new ones to backend servers.
- Can route based on request content (URL, headers, cookies, etc.).
- More CPU-intensive due to packet inspection.
- Provide more flexibility and features.
- Better suited for HTTP-based traffic.

For example, an L7 load balancer could:
- route all API requests to one set of servers while sending web page requests to another (providing similar functionality to an [API Gateway](https://www.hellointerview.com/learn/system-design/deep-dives/api-gateway)),
- it could ensure that all requests from a specific user go to the same server based on a cookie.
- The underlying TCP connection that's made to your server via an L7 load balancer is not all that relevant! It's just a way for the load balancer to forward L7 requests, like HTTP, to your server.

While L7 load balancers can help us to not have to worry about lower-level details like TCP connections, we aren't able to ignore the connection-level reality if we want persistent connections to consistent servers.

###### Where to Use It

Layer 7 load balancers are great for HTTP-based traffic which is going to cover all of the protocols we've discussed so far except for Websockets.

> [!info]
> There are some L7 load balancers which explicitly support connection-oriented protocols like WebSockets,
>
> but generally speaking L4 load balancers are better for WebSocket connections, while L7 load balancers offer more flexibility for HTTP-based solutions like long polling.

##### Health Checks and Fault Tolerance

1) While load balancers play a key role in distributing load and traffic, they are also responsible for monitoring the health of backend servers.
2) If a server loses power or crashes, the load balancer stops routing traffic to it until it recovers.
3) This automatic failover capability is what makes load balancers essential for high availability.
4) They can detect and route around failures without user intervention.
5) To do this, load balancers use **health checks**.
	- Health checks are a way for the load balancer to determine if a server is healthy.
	- They can be configured to check the server at different intervals and with different protocols.
	- Health checks can be configured to check the server at different intervals and with different protocols.
	- A common approach is to use a TCP health check, which is a simple and efficient way to check if a server is accepting new connections.
	- A Layer 7 health check might make an HTTP request to the server and make sure the response is success (e.g. a 200 status code vs a 500 indicating internal failures or no response indicating a crash).


##### Load Balancing Algorithms

The other benefit of a dedicated load balancer is that we have more choices over the algorithm used to distribute traffic.

Several options are available with most load balancers:

- **Round Robin**: Requests are distributed sequentially across servers
- **Random**: Requests are distributed randomly across servers
- **Least Connections**: Requests go to the server with the fewest active connections
- **Least Response Time**: Requests go to the server with the fastest response time
- **IP Hash**: Client IP determines which server receives the request (useful for session persistence)

1) Usually, a round robin or random algorithm is appropriate, especially for stateless applications where we don't expect any particular server to be more popular than any other.
2) When a new server is introduced to the load balancer (e.g. for scaling), these algorithms will naturally start distributing traffic to it without any special configuration.
3) For services that require a persistent connection (e.g. those serving SSE or WebSocket connections), using Least Connections is a good idea because it avoids a situation where a single server gradually accumulates all of of the active connections.

##### Real-World Implementations

In practice, you'll encounter dedicated load balancers in various forms:

- **Hardware Load Balancers**: Physical devices like F5 Networks BIG-IP
- **Software Load Balancers**: HAProxy, NGINX, Envoy
- **Cloud Load Balancers**: AWS ELB/ALB/NLB, Google Cloud Load Balancing, Azure Load Balancer

Enterprise hardware load balancers can scale to support 100's of millions of requests per second, whereas software load balancers are more limited.

If you find the load balancer throughput is large — mentioning **hardware load balancers** is a good way out.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[WebSockets]] — requires L4 load balancer for persistent connections
- [[Server-Sent Events]] — works with L7 load balancers
- [[gRPC]] — has built-in client-side load balancing
- [[Regionalization and Latency]] — load balancers + CDNs for global scale
- [[Circuit Breakers]] — pair with LB health checks for fault tolerance
