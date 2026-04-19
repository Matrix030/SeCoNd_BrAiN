---
tags: [system-design, hld, networking, udp]
aliases: ["User Datagram Protocol", "UDP Protocol"]
---

# UDP: Fast but Unreliable

- User Datagram Protocol (UDP) is the machinegun of protocols. It offers few features on top of IP but is very fast. **Spray and pray** is the right way to think about this.
- It provides a simpler, connectionless service with no guarantees of delivery, ordering, or duplicate protection.
- If you write an application that receives UDP datagrams, you'll be able to see where they came from (i.e. the source IP address and port) and where they're going (i.e. the destination IP address and port). But that's it! The rest is a binary blob.

#### Key characteristics of UDP include:

1. **Connectionless**: No handshake or connection setup
2. **No guarantee of delivery**: Packets may be lost without notification
3. **No ordering**: Packets may arrive in a different order than sent
4. **Lower latency**: Less overhead means faster transmission

No setup sounds great but 2 and 3 kinda suck, so why would you want to use UDP?

1) UDP is perfect for applications where **speed is more important than reliability**, such as live video streaming, online gaming, VoIP, and DNS lookups.
2) In these cases the application or client is equipped to handle the occasional packet loss or out of order packet.
3) For VOIP as an example, the client might just drop the occasional packet leading to a hiccup in the audio but overall the conversation is still intelligible. This is vastly preferable to retransmitting those lost packets and clogging up the network with ACKs.

> [!warning]
> Browsers don't have widespread support for UDP yet outside of WebRTC. If you're thinking about a design which could use UDP (like the spamming of hearts and reactions in [Facebook Live Comments](https://www.hellointerview.com/learn/system-design/problem-breakdowns/fb-live-comments)), think about what you'll do for your browser-based users. It might be that your app-based users get a real-time UDP stream of reactions while browser-based users a slower, batched HTTP stream which you spread out over time in the UI.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[TCP]] — the reliable alternative
- [[Transport Layer Protocols]] — when to choose UDP vs TCP
- [[WebRTC]] — the main browser-based UDP use case
