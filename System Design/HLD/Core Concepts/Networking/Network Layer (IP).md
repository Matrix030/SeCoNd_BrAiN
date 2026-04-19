---
tags: [system-design, hld, networking]
aliases: ["IP Protocol", "Network Layer", "IP Addressing"]
---

# Network Layer (IP)

 - This layer is dominated by the IP protocol, which is responsible for routing and addressing.
 - In a system, nodes are assigned IPs usually by a [DHCP server](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) when they boot up.
 - These IP addresses are arbitrary and only mean something in as much as we tell people about them.
 - If I want to, I can create a private network with my servers and give them any IP address I want, but if you want internet traffic to be able to find them you'll need to use IP addresses that are routable and allocated by a [RIR](https://en.wikipedia.org/wiki/Regional_Internet_Registry).
 - These assigned IP addresses are called [public IPs](https://en.wikipedia.org/wiki/Public_IP_address) and are used to identify devices on the internet. The most important thing about them is that internet routing infrastructure is optimized to route traffic between public IPs and knows where they are.
 - Any address starting with 17 (e.g. 17.0.0.0) is part of Apple — the backbone of the internet knows that when you want to send a packet to these addresses, you need to send it to their routers.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[Networking Layers]] — where Layer 3 fits in the OSI model
- [[Transport Layer Protocols]] — Layer 4 builds on top of IP
