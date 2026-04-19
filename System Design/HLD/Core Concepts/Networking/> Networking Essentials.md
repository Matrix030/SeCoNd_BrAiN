---
tags: [system-design, hld, networking, moc]
aliases: ["Networking", "Networking 101"]
---

# Networking Essentials — Map of Content

> [!info] About this section
> Core networking knowledge for system design interviews — from OSI layers up through application protocols, load balancing, and failure handling.

1) At its core, networking is about connecting devices and enabling them to communicate.
2) Networks are built on a layered architecture (the so-called ["OSI model"](https://en.wikipedia.org/wiki/OSI_model)) which greatly simplifies the world for us application developers who sit on top of it.

---

## Fundamentals

- [[Networking Layers]] — OSI model overview: Layer 3 (IP), Layer 4 (TCP/UDP), Layer 7 (HTTP/DNS)
- [[A Simple Web Request]] — how DNS, TCP, and HTTP work together end-to-end
- [[Network Layer (IP)]] — IP addressing, DHCP, public vs private IPs

## Transport Layer

- [[Transport Layer Protocols]] — TCP vs UDP overview and when to choose each
- [[TCP]] — reliable, ordered, connection-oriented delivery
- [[UDP]] — fast, connectionless, best-effort delivery

## Application Layer Protocols

> [!info]
> Typically the application layer is processed in ["User Space"](https://en.wikipedia.org/wiki/User_space_and_kernel_space) whereas layers beneath it are processed in the OS kernel in "Kernel Space". This means that the application layer is more flexible and can be more easily modified than lower layers, whereas lower layers are difficult to change but can be _very_ efficient.

- [[HTTP and HTTPS]] — request-response, methods, status codes, headers
- [[REST]] — resource-oriented API design over HTTP
- [[GraphQL]] — flexible data fetching for complex clients
- [[gRPC]] — efficient binary RPC for internal service-to-service communication
- [[Server-Sent Events]] — server push over a single HTTP connection
- [[WebSockets]] — persistent bidirectional connections
- [[WebRTC]] — peer-to-peer audio/video

## Scaling and Infrastructure

- [[Load Balancing]] — client-side vs dedicated, L4 vs L7, algorithms, health checks
- [[Regionalization and Latency]] — CDNs, regional partitioning, data locality

## Fault Tolerance

- [[Timeouts Retries and Backoff]] — retries, exponential backoff, jitter, idempotency keys
- [[Circuit Breakers]] — preventing cascading failures

> [!tip] Interview hierarchy
> Start with the transport layer choice ([[TCP]] vs [[UDP]]), then pick the right application protocol, then think about scaling ([[Load Balancing]]) and fault tolerance ([[Circuit Breakers]]).
