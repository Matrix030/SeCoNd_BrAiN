---
tags: [system-design, hld, real-time, http]
aliases: ["Long Polling"]
---

# Long Polling: The Easy Solution
1) After a baseline for simple polling, long polling is the easiest approach to achieving near real-time updates.
2) It builds on standard HTTP, making it easy to implement and scale.
3) The idea is also simple: the client makes a request to the server and the server holds the request open until new data is available. 
4) It's as if the server is just taking really long to process the request. 
5) The server then responds with the data, finalizes the HTTP requests, and the client immediately makes a **new** HTTP request.
6) This repeats until the server has new data to send.
7) If no data has come through in a long while, we might even return an empty response to the client so that they can make another request.

For our chat app, we would keep making a request to get the _next message_. If there was no message to retrieve, the server would just hold the request open until a new message was sent before responding to us. After we received that message, we'd make a new request for the next message.

1. Client makes HTTP request to server
2. Server holds request open until new data is available
3. Server responds with data
4. Client immediately makes new request
5. Process repeats

```
// Client-side of long polling
async function longPoll() {
  while (true) {
    try {
      const response = await fetch('/api/updates');
      const data = await response.json();
      
      // Handle data
      processData(data);
    } catch (error) {
      // Handle error
      console.error(error);
      
      // Add small delay before retrying on error
      await new Promise(resolve => setTimeout(resolve, 1000));
    }
  }
}
```

The simplicity of the approach hides an **important trade-off** for high-frequency updates. Since the client needs to "call back" to the server after each receipt, the approach can introduce some extra latency:
![[Pasted image 20260524164742.png]]

> _Assume the latency between the client and server is 100ms._
> 
> _If we have 2 updates which occur within 10ms of each other, with long polling we'll receive the first update 100ms after it occurred (100ms of network latency) but the second update may be up to 290ms after it occurred (90ms for the first response to finish returning, 100ms for the second request to be made, and another 100ms to get the response)._

## Advantages

- Builds on standard HTTP and works everywhere HTTP works.
- Easy to implement.
- No special infrastructure needed.
- Stateless server-side.

## Disadvantages

- Higher latency than alternatives.
- More HTTP overhead.
- Can be resource-intensive with many clients.
- Not suitable for frequent updates due to the issues mentioned above.
- Makes monitoring more painful since requests can hang around for a long time.
- Browsers limit the number of concurrent connections per domain, meaning you may only be able to have a few long-polling connections per domain.

## When to Use Long Polling

1) Long polling is a great solution for near real-time updates with a simple implementation.
2) It's a good choice when updates are infrequent and a simple solution is preferred.
3) If the latency trade-off of a simple polling solution is at all an issue, long-polling is an obvious upgrade with minimal additional complexity.
4) Long Polling is a great solution for applications where a long async process is running but you want to know when it finishes, as soon as it finishes - like is often the case in payment processing.

## Things to Discuss in Your Interview
1) Because long-polling utilizes the existing HTTP infrastructure, there's not a bunch of extra infrastructure you're going to need to talk through.
2) Even though the polling is "long", you still do need to be specific about the polling frequency.
3) Keep in mind that each hop in your infrastructure needs to be aware of these lengthy requests: you don't want your load balancer hanging up on the client after 30 seconds when your long-polling server is happy to keep the connection open for 60 (15-30s is a pretty common polling interval that minimizes the fuss here).

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Simple Polling]] — the simpler baseline this builds on
- [[Server-Sent Events (SSE)]] — the streaming upgrade that fixes high-frequency latency
- [[Choosing a Protocol]] — where long polling fits among the options
