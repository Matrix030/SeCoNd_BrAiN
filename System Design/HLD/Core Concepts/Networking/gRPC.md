---
tags: [system-design, hld, networking, api, grpc]
aliases: ["gRPC", "Protocol Buffers", "protobuf", "Remote Procedure Call"]
---

# gRPC: Efficient Service Communication

1) gRPC is a high-performance RPC (Remote Procedure Call) framework from Google (the "g") that uses HTTP/2 and Protocol Buffers.
2) Think of Protocol Buffers like JSON but with a more rigid schema that allows for better performance and more efficient serialization. Here's an example of a Protocol Buffer definition for a User resource:

```
message User {
  string id = 1;
  string name = 2;
}
```

Instead of a chunky JSON object with embedded schema (40 bytes)

```
{
  "id": "123",
  "name": "John Doe"
}
```

we have a binary encoding (15 bytes) of the same data with very skinny tags and variable length encoding of the strings. Less space and less CPU to parse!

```
0A 03 31 32 33 12 08 6A 6F 68 6E 20 64 6F 65
```

gRPC builds on this to provide service definitions. Here's an example of a gRPC service definition for a UserService:

```
message GetUserRequest {
  string id = 1;
}

message GetUserResponse {
  User user = 1;
}

service UserService {
  rpc GetUser (GetUserRequest) returns (GetUserResponse);
}
```

3) These definitions are compiled into a client and server stub which a wide variety of languages and frameworks can consume to build services and clients.
4) gRPC includes a bunch of features relevant for operating microservice architectures at scale (it was invented by Google after all) like streaming, deadlines, client-side load balancing and more.
5) But the most important thing to know is that it's a binary protocol that's faster and more efficient than JSON over HTTP.

##### Where to Use It

1) gRPC shines in microservices architectures where services need to communicate efficiently.
2) Its strong typing helps catch errors at compile time rather than runtime, and its binary protocol is more efficient than JSON over HTTP ([some benchmarks show a factor of 10x throughput!](https://medium.com/@i.gorton/scaling-up-rest-versus-grpc-benchmark-tests-551f73ed88d4)).
3) Consider gRPC for internal service-to-service communication, especially when performance is critical or when latencies are dominated by the network rather than the work the server is doing.
4) That said, you generally won't use gRPC for public-facing APIs, especially for clients you don't control, because it's a binary protocol and the tooling for working with it is less mature than simple JSON over HTTP.
5) Having internal APIs using gRPC and external APIs using REST is a great way to get the benefits of a binary protocol without the complexity of a public-facing API.
6) There are definitely engineers who would love it if gRPC was more widely adopted, but it's not there yet.

![[Pasted image 20260418203954.png]]

**it is recommended using REST for public-facing APIs and leaving gRPC for internal service-to-service communication** — especially if binary data is being exchanged or performance is critical. In many interviews, using REST both for internal and external APIs is fine and you can build from there depending on the needs of the problem and probes from your interviewer.

> [!tip]
> Sometimes engineers think the point of a system design interview is to draw up an optimal solution to a problem on a whiteboard. But interviewers typically are trying to understand how you think through a problem and how you react to challenges and constraints you may not have seen before. Be wary of hyperoptimizing your RPC protocol choice before you've handled other substantial bottlenecks in the problem. Premature optimization is the root of all evil!

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[REST]] — recommended for public-facing APIs
- [[GraphQL]] — alternative for flexible data fetching
- [[Load Balancing]] — gRPC has built-in client-side load balancing
