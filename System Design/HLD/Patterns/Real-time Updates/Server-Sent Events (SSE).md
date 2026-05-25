---
tags: [system-design, hld, real-time, http]
aliases: ["SSE One-Way Street", "Server-Sent Events for Real-time"]
---

# Server-Sent Events (SSE): The Efficient One-Way Street
1) SSE is an extension on long-polling that allows the server to send a stream of data to the client.
2) Normally HTTP responses have a header like Content-Length which tells the client how much data to expect. SSE instead uses a special header Transfer-Encoding: chunked which tells the client that the response is a series of chunks - we don't know how many there are or how big they are until we send them.
3) This allows us to move from a single, atomic request/response to a more granular "stream" of data.
4) With SSE, instead of sending a full response once data becomes available, the server sends a chunk of data and then keeps the request open to send more data as needed.
5) SSE is perfect for scenarios where servers need to push data to clients, but clients don't need to send data back frequently.

In our chat app, we would open up a request to stream messages and then each new message would be sent as a chunk to the client.

## How SSE Works

1. Client establishes SSE connection
2. Server keeps connection open
3. Server sends messages when data changes or updates happen
4. Client receives updates in real-time

Modern browsers have built-in support for SSE through the EventSource object, making the client-side implementation straightforward.

```
// Client-side
const eventSource = new EventSource('/api/updates');

eventSource.onmessage = (event) => {
  const data = JSON.parse(event.data);
  updateUI(data);
};

// Server-side (Node.js/Express example)
app.get('/api/updates', (req, res) => {
  res.setHeader('Content-Type', 'text/event-stream');
  res.setHeader('Cache-Control', 'no-cache');
  res.setHeader('Connection', 'keep-alive');

  const sendUpdate = (data) => {
    res.write(`data: ${JSON.stringify(data)}\n\n`);
  };

  // Send updates when data changes
  dataSource.on('update', sendUpdate);

  // Clean up on client disconnect
  req.on('close', () => {
    dataSource.off('update', sendUpdate);
  });
});
```

## Advantages

- Built into browsers.
- Automatic reconnection.
- Works over HTTP.
- More efficient than long polling due to less connection initiation/teardown.
- Simple to implement.

## Disadvantages

- One-way communication only.
- Limited browser support (not an issue for modern browsers).
- Some proxies and networking equipment don't support streaming. Nasty to debug!
- Browsers limit the number of concurrent connections per domain, meaning you may only be able to have a few SSE connections per domain.
- Makes monitoring more painful since requests can hang around for a long time.

## When to Use SSE

1) SSE is a great upgrade to long-polling because it eliminates the issues around high-frequency updates while still building on top of standard HTTP.
2) That said, it comes with lesser overall support because you'll need both browsers and all infra between the client and server to support streaming responses.
3) A very popular use-case for SSE today is AI chat apps which frequently involve the need to stream new tokens (words) to the user as they are generated to keep the UI responsive.
4) An example of an infra gap is that many proxies and load balancers don't support streaming responses. In these cases, the proxy will try to buffer the response until it completes - which effectively blocks our stream in an annoying, opaque way that is hard to debug! 

## Things to Discuss 
1) SSE rides on existing HTTP infrastructure, so there's not a lot of extra infrastructure you'll need to talk through. 
2) You also don't have a polling interval to negotiate or tune.
3) Most SSE connections won't be super-long-lived (e.g. 30-60s is pretty typical), so if you need to send messages for a longer period you'll need to talk about how clients re-establish connections and how they deal with the gaps in between.
4) The [SSE standard](https://html.spec.whatwg.org/multipage/server-sent-events.html) includes a "last event ID" which is intended to cover this gap, and the EventSource object in browsers explicitly handles this reconnection logic. 
5) If a client loses its connection, it can reconnect and provide the last event ID it received. 
6) The server can then use that ID to send all the events that occurred while the client was disconnected.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Server-Sent Events]] — the protocol fundamentals (networking note)
- [[Long Polling]] — what SSE extends
- [[WebSockets - Full-Duplex Champion]] — when clients also need to push frequently
- [[Choosing a Protocol]] — where SSE fits among the options
