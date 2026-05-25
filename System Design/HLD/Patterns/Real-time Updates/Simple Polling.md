---
tags: [system-design, hld, real-time, http]
aliases: ["Simple Polling", "Polling"]
---

# Simple Polling: The Baseline

1) The simplest possible approach is to have the client regularly poll the server for updates.
2) This could be done with a simple HTTP request that the client makes at a regular interval.
3) This doesn't technically qualify as real-time, but it's a good starting point and provides a good contrast for our other methods.

> [!tip]
> A lot of questions don't _actually_ require real-time updates. Think critically about the product and ask yourself whether lower frequency updates (e.g. every 2-5 seconds) would work. If so, you may want to propose a simple, polling-based approach. It's better to suggest a less-than-perfect solution than to fail to implement a complex one.


4) The client makes a request to the server at a regular interval and the server responds with the current state of the world. 
5) In our chat app, we would just constantly be polling for "what messages have I not received yet?".

```
async function poll() {
  const response = await fetch('/api/updates');
  const data = await response.json();
  processData(data);
}

// Poll every 2 seconds
setInterval(poll, 2000);
```

## Advantages

- Simple to implement.
- Stateless.
- No special infrastructure needed.
- Works with any standard networking infrastructure.
- Doesn't take much time to explain.

> [!tip]
> This last point is underrated. If the crux of your problem is _not_ real-time updates, you'll want to propose a simple, polling-based approach. You'll preserve your mental energy and interview time for the parts of the system that truly matter.

## Disadvantages

- Higher latency than other methods. Updates might be delayed as long as the polling interval + the time it takes to process the request.
- Limited update frequency.
- More bandwidth usage.
- Can be resource-intensive with many clients, establishing new connections, etc.

## When to use simple polling
Simple polling is a great baseline and, unless you specifically require very-low latency, real-time updates, it's a great solution. It's also appropriate when the window where you need updates is short, like in our [Leetcode system design](https://www.hellointerview.com/learn/system-design/problem-breakdowns/leetcode).
## Things to Discuss

1) You'll want to be clear about the trade-offs you're making with polling vs other methods. 
2) A good explanation highlights the simplicity of the approach and gives yourself a backdoor if you discover that you need something more sophisticated.
3) "I'm going to start with a simple polling approach so I can focus on X, but we can switch to something more sophisticated if we need to later."
4) The most common objection from interviewers to polling is either that it's too slow or that it's not efficient.
5) Be prepared to discuss why the polling frequency you've chosen is appropriate and sufficient for the problem. 
6) On the efficiency front, it's great to be able to discuss how you can reduce the overhead. 
7) One way to do this is to take advantage of HTTP keep-alive connections.
8) Setting an HTTP keep-alive which is longer than the polling interval will mean that, in most cases, you'll only need to establish a TCP connection once which minimizes some of the setup and teardown overhead.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Long Polling]] — the near-real-time upgrade with minimal extra complexity
- [[Pulling with Simple Polling]] — the matching server-side (pull) trigger
- [[Choosing a Protocol]] — where polling fits among the options
