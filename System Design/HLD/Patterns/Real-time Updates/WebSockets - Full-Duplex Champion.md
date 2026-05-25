---
tags: [system-design, hld, real-time, websockets]
aliases: ["WebSockets Full-Duplex", "WebSockets for Real-time"]
---

# Websockets: The Full-Duplex Champion
1) WebSockets are the go-to choice for true bi-directional communication between client and server.
2) If you have high frequency writes _and_ reads, WebSockets are the champ.

## How WebSockets Works

1) Websockets build on HTTP through an "upgrade" protocol, which allows an existing TCP connection to change L7 protocols. 
2) This is super convenient because it means you can utilize some of the existing HTTP session information (e.g. cookies, headers, etc.) to your advantage.
> [!warning]
> Just because clients can upgrade from HTTP to WebSocket doesn't mean that the infrastructure will support it. Every piece of infrastructure between the client and server will need to support WebSocket connections.

3) Once a connection is established, both client and server can send "messages" to each other which are effectively opaque binary blobs. You can shove strings, JSON, Protobufs, or anything else in there. Think of WebSockets like a TCP connection with some niceties that make establishing the connection easier, especially for browsers.

4. Client initiates WebSocket handshake over HTTP
5. Connection upgrades to WebSocket protocol
6. Both client and server can send messages
7. Connection stays open until explicitly closed

For our chat app, we'd connect to a WebSocket endpoint over HTTP, sharing our authentication token via cookies. The connection would get upgraded to a WebSocket connection and then we'd be able to receive messages back to the client over the connection as they happen. Bonus: we'd also be able to send messages to other users in the chat room!

```
// Client-side
const ws = new WebSocket('ws://api.example.com/socket');

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  handleUpdate(data);
};

ws.onclose = () => {
  // Implement reconnection logic
  setTimeout(connectWebSocket, 1000);
};

// Server-side (Node.js/ws example)
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
  ws.on('message', (message) => {
    // Handle incoming messages
    const data = JSON.parse(message);
    processMessage(data);
  });

  // Send updates to client
  dataSource.on('update', (data) => {
    ws.send(JSON.stringify(data));
  });
});
```

## Extra Challenges

1) Because the Websocket is a _persistent connection_, we need our infrastructure to support it.
2) Some L7 load balancers support websockets, but support is generally spotty (remember that L7 load balancers aren't guaranteeing we're using the same TCP connection for each incoming request). 
3) L4 load balancers will support websockets natively since the same TCP connection is used for each request.
4) When we have long-running connections we have another problem: deployments. 
5) When servers are redeployed, we either need to sever all old connections and have them reconnect or have the new servers take over and keep the connections alive. 
6) Generally speaking you should prefer the former since it's simpler, but it does have some ramifications on how "persistent" you expect the connection to be. 
7) You also need to be able to handle situations where a client needs to reconnect and may have missed updates while they were disconnected.
8) Finally, balancing load across websocket servers can be more complex. If the connections are truly long-running, we have to "stick with" each allocation decision we made.
9) If we have a load balancer that wants to send a new request to a different server, it can't do that if it would break an existing websocket connection.

Because of all the issues associated with _stateful_ connections, many architectures will terminate WebSockets into a WebSocket service early in their infrastructure. This service can then handle the connection management and scaling concerns and allows the rest of the system to remain _stateless_. The WebSocket service is also less likely to change meaning it requires less deployments which churn connections.

![[Pasted image 20260524182119.png]]

## Advantages
- Full-duplex (read and write) communication.
- Lower latency than HTTP due to reduced overhead (e.g. no headers).
- Efficient for frequent messages.
- Wide browser support.

## Disadvantages
- More complex to implement.
- Requires special infrastructure.
- Stateful connections, can make load balancing and scaling more complex.
- Need to handle reconnection.

## When to Use WebSockets
1) Generally speaking, if you need **high-frequency**, bi-directional communication, you're going to want to use WebSocket. 
2) I'm emphasizing high-frequency here because you can always make additional requests/connections for writes: a very common pattern is to have SSE subscriptions for updates and do writes over simple HTTP POST/PUT whenever they occur.
3) I often find candidates too eagerly adopting Websockets when they could be using SSE or simple polling instead.
4) Because of the additional complexity and infra lift, you'll want to defer to SSE unless you have a specific need for this bi-directional communication.
## Things to Discuss
1) Websockets are a powerful tool, but they do come with a lot of complexity. 
2) You'll want to talk through how you'll manage connections and deal with reconnections. 
3) You'll also need to consider how your deployment strategy will handle server restarts.
4) Managing _statefulness_ is a big part of the conversation.
5) There's also a lot to discuss about how to scale WebSocket servers. 
6) Load can be uneven which can result in hotspots and failures. Using a **"least connections"** strategy for the load balancer can help, as well as minimizing the amount of work the WebSocket servers need to do as they process messages. 
7) Using the reference architecture above and offloading more intensive processing to other services (which can scale independently) can help.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[WebSockets]] — the protocol fundamentals (networking note)
- [[Server-Sent Events (SSE)]] — the simpler one-way alternative to defer to
- [[Pushing via Consistent Hashes]] — managing stateful WebSocket connections at scale
- [[WebRTC - Peer-to-Peer]] — peer-to-peer alternative for audio/video
