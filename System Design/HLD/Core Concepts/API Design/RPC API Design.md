---
tags: [system-design, hld, api]
aliases: ["RPC", "gRPC API", "Remote Procedure Call"]
---

# RPC API Design

1) RPC (Remote Procedure Call) is a communication paradigm that allows a client to call a procedure on a server and wait for a response without the client having to understand the underlying network details.
2) Protocols like [[gRPC]] use binary serialization and HTTP/2, making them faster than traditional JSON-over-HTTP [[REST API Design|REST]] APIs for service communication.

## How RPC Works

1) Unlike REST's resource-oriented approach, RPC is action-oriented.
2) You're essentially calling functions across a network as if they were local functions in your codebase.
3) Here's how the same Ticketmaster operations might look with RPC:

```
// Instead of GET /events/123
getEvent(eventId: "123")

// Instead of POST /events/123/bookings
createBooking(eventId: "123", userId: "456", tickets: [...])

// Instead of GET /events/123/tickets
getAvailableTickets(eventId: "123", section: "VIP")
```

4) The most popular RPC protocol today is [[gRPC]], which uses Protocol Buffers for serialization and HTTP/2 for transport. 
5) This combo is much faster than REST's JSON-over-HTTP approach, especially for service-to-service communication. 
6) Another notable RPC framework is Apache Thrift, originally developed at Facebook and now open source, which supports multiple programming languages and serialization formats.

## Protocol Buffers and Type Safety

1) gRPC uses Protocol Buffers (protobuf) to define service contracts. You write a .proto file that describes your service methods and data structures:

```
service TicketService {
  rpc GetEvent(GetEventRequest) returns (Event);
  rpc CreateBooking(CreateBookingRequest) returns (Booking);
  rpc GetAvailableTickets(GetTicketsRequest) returns (TicketList);
}

message GetEventRequest {
  string event_id = 1;
}

message Event {
  string id = 1;
  string name = 2;
  int64 date = 3;
  Venue venue = 4;
}
```

2) From this single definition, gRPC generates client and server code in multiple programming languages. This means your Go backend service and your Java payment service can communicate with compile-time type safety, catching mismatches before deployment.

## When to Use RPC

1) RPC shines in microservice architectures where services need to communicate frequently and efficiently.
2) If you want internal service communication, high-performance requirements, or **polyglot** environments (different services in different languages), RPC is likely a good choice.

Consider RPC when:

- **Performance is critical**: Binary serialization and HTTP/2 make RPC significantly faster than JSON REST
- **Type safety matters**: Generated client code prevents many runtime errors
- **Service-to-service communication**: Internal APIs between your own services don't need REST's resource semantics
- **Streaming is needed**: gRPC supports bidirectional streaming for real-time features

For the Ticketmaster example, you might use REST APIs for your public endpoints that mobile apps and web clients consume, but use gRPC for internal communication between your booking service, payment service, and inventory service.

> [!info]
> Unless explicitly asked, you won't typically outline your internal APIs during the API step of the interview. Instead, focus on just the user facing APIs here. At most, you'll call out that internal services communicate over RPC during your high-level design.

## Related

- [[> API Design]] — back to the API Design section MOC
- [[REST API Design]] — resource-oriented alternative for public-facing APIs
- [[GraphQL API Design]] — flexible query alternative for diverse client needs
- [[gRPC]] — deeper look at the protocol itself
- [[Networking Essentials]] — HTTP/2 and binary serialization context
