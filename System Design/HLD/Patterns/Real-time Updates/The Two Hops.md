---
tags: [system-design, hld, real-time]
aliases: ["Real-time Updates Overview", "Two Hops"]
---

# The Two Hops

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

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Networking 101]] — the networking primer behind the first hop
- [[Server-Side Push and Pull]] — the second hop: source → server
- [[Simple Polling]] — the simplest first-hop baseline
