---
tags: [system-design, hld, networking, http, real-time]
aliases: ["SSE", "EventSource", "Server-Sent Events"]
---

# Server-Sent Events (SSE): Real-Time Push Communication

Server-Sent Events (SSE) is a spec defined on top of HTTP that allows a server to push many messages to the client over a single HTTP connection.

Here's how to think of it:
1) SSE is a nice hack on top of HTTP that **allows a server to stream many messages, over time, in a single response from the server**.
2) With most HTTP APIs you'd get a single, cohesive JSON blob as a response from the server that is processed once the whole thing has been received.

```
{
  "events": [
    { "id": 1, "timestamp": "2025-01-01T00:00:00Z", "description": "Event 1" },
    { "id": 2, "timestamp": "2025-01-01T00:00:01Z", "description": "Event 2" },
    ...
    { "id": 100, "timestamp": "2025-01-01T00:00:10Z", "description": "Event 100" }
  ]
}
```

3) Since we have to wait for the whole response to come in before we can process it, it's not much good for push notifications!
4) On the other hand, with SSE, the server can push many messages as "chunks" in a single response from the server:

```
data: {"id": 1, "timestamp": "2025-01-01T00:00:00Z", "description": "Event 1"}
data: {"id": 2, "timestamp": "2025-01-01T00:00:01Z", "description": "Event 2"}
...
data: {"id": 100, "timestamp": "2025-01-01T00:00:10Z", "description": "Event 100"}
```

5) Each line here is received as a separate message from the server. The client can then process each message as it comes in. It's still one big HTTP response (same TCP connection), but it comes in over many smaller packets and clients are expected to process each line of the body individually to allow them to react to the data as it comes in.

Now with all good hacks, SSE comes with some **acute limitations**.
1) We can't keep an SSE connection open for too long because the server (or the load balancer, or a middle box proxy) will close down the connection.
2) So the SSE standard defines the behavior of an EventSource object that, once the connection is closed, will automatically reconnect with the ID of the last message received.
3) Servers are expected to keep track of prior messages that may have been missed while the client was disconnected and resend them.
4) In practice there are also some nasty, misbehaving networks that will batch up all SSE responses into a single response [making it behave a lot like what we're trying to avoid](https://dev.to/miketalbot/server-sent-events-are-still-not-production-ready-after-a-decade-a-lesson-for-me-a-warning-for-you-2gie).

##### Where to Use It

1) Situations where you want clients to get notifications or events as soon as they happen.
2) SSE is a great option for [keeping bidders up-to-date on the current price of an auction](https://www.hellointerview.com/learn/system-design/problem-breakdowns/online-auction#3-how-can-we-ensure-that-the-system-displays-the-current-highest-bid-in-real-time), for example.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[HTTP and HTTPS]] — SSE is built on top of HTTP
- [[WebSockets]] — bidirectional alternative when clients also need to push
- [[Load Balancing]] — L7 load balancers handle SSE connections
