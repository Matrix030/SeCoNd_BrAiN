---
tags: [system-design, hld, networking, tcp, udp]
aliases: ["Transport Layer", "TCP vs UDP"]
---

# Transport Layer Protocols

- The transport layer is where we establish end-to-end communication between applications.
- They give us some guarantees instead of handing us a jumbled mess of packets.
- The three primary protocols at this layer are TCP, UDP, and QUIC, each with distinct characteristics that make them suitable for different use cases.
- For most system design interviews, the real choice you'll be faced with is **between [[TCP]] and [[UDP]]**.
- QUIC is a new protocol that aims to provide some of the same benefits of TCP with some modernization and performance benefits.
- While QUIC is becoming more popular, it's still a relatively new protocol and not yet ubiquitous - for our purposes we'll consider it a better version of TCP but without the same broad baseline of adoption.

### When to Choose Each Protocol

But you'll be able to earn extra points if you can make the case for a UDP application and not bungle the details. So the question you should be asking yourself is whether UDP is a better fit for your use-case.

You might choose **UDP** when:

- Low latency is critical (real-time applications, gaming)
- Some data loss is acceptable (media streaming)
- You're handling high-volume telemetry or logs where occasional loss is acceptable
- You don't need to support web browsers (or you have an alternative for that client)

> [!info]
> Modern applications often use both protocols. For example, a web-based video conferencing app might use TCP/HTTP for signaling and authentication but UDP/WebRTC for the actual audio/video streams.

#### TCP vs UDP Comparison

| Feature            | UDP                     | TCP                    |
| ------------------ | ----------------------- | ---------------------- |
| Connection         | Connectionless          | Connection-oriented    |
| Reliability        | Best-effort delivery    | Guaranteed delivery    |
| Ordering           | No ordering guarantees  | Maintains order        |
| Flow Control       | No                      | Yes                    |
| Congestion Control | No                      | Yes                    |
| Header Size        | 8 bytes                 | 20-60 bytes            |
| Speed              | Faster                  | Slower due to overhead |
| Use Cases          | Streaming, gaming, VoIP | Everything Else        |

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[TCP]] — deep dive on TCP
- [[UDP]] — deep dive on UDP
- [[Networking Layers]] — where Layer 4 fits in the OSI model
