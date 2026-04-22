---
tags: [system-design, hld, api]
aliases: ["GraphQL"]
---

# GraphQL API Design

1) GraphQL emerged from Facebook in 2012 to solve a specific problem: their mobile app needed different data than their web app, but they were stuck with REST endpoints that returned fixed data structures.
2) The mobile team kept asking for new endpoints or modifications to existing ones and this was slowing down development on both sides.
3) With [[REST API Design|REST]], you typically have two unpleasant choices when different clients need different data.
4) You can create multiple endpoints for different use cases, leading to endpoint proliferation and maintenance headaches.
5) Or you can make your endpoints return everything any client might need, leading to over-fetching where mobile clients download megabytes of data they don't use.
6) GraphQL consolidates resource endpoints into a single endpoint that accepts queries describing exactly what data you want. 
7) The client specifies the shape of the response, and the server returns data in that exact format.

## How GraphQL Works

1) Here's a simple example using Ticketmaster scenario. Instead of separate REST endpoints for events, venues, and tickets, you'd have a single GraphQL endpoint that can handle queries like this:

```
query {
  event(id: "123") {
    name
    date
    venue {
      name
      address
    }
    tickets {
      section
      price
      available
    }
  }
}
```

2) The server returns exactly what you asked for, nothing more, nothing less. 
3) If the mobile app only needs event names and dates, it can request just those fields.
4) If the web dashboard needs comprehensive event details with venue information and ticket availability, it can request all of that in a single query.

## When to Use GraphQL

1) GraphQL is the right choice when you have diverse clients with different data needs. 
2) If there is a scenarios like 
	- "the mobile app needs different data than the web app" or 
	- "avoiding over-fetching and under-fetching,"
	it's more likely that you need to use GraphQL.

3) It's also a good choice when frontend teams need to iterate quickly without backend changes. 
4) With REST, adding a new field to a mobile screen often requires backend changes and API deployments.
5) With GraphQL, the frontend team can request additional fields as long as they exist in the schema.
6) However, GraphQL **adds complexity**.
7) You need to implement query parsing, schema validation, and often sophisticated caching strategies. 
8) For most system designs, REST is simpler and more straightforward unless the problem specifically calls for GraphQL's flexibility.

## GraphQL Schema Design

1) If you do choose GraphQL, you'll need to think differently about design.
2) Instead of REST's resource endpoints, you design a schema that defines your data types and their relationships.

For our Ticketmaster example, you'd start by modeling your core entities as GraphQL types:

```
type Event {
  id: ID!
  name: String!
  date: DateTime!
  venue: Venue!
  tickets: [Ticket!]!
}

type Venue {
  id: ID!
  name: String!
  address: String!
}

type Query {
  event(id: ID!): Event
  events(limit: Int, after: String): [Event!]!
}
```

3) The key difference from REST is that you define relationships directly in the schema. 
4) An Event has a Venue, and clients can traverse that relationship in a single query.
5) But this flexibility creates the **N+1 problem**, the biggest GraphQL gotcha.
	- When a client queries events with their venues, you might execute one query for events, then N separate queries for each venue. With 100 events, that's 101 database queries instead of 2.
	- The solution is batching/dataloader patterns that group related queries together, but it adds complexity you don't have with REST.

6) GraphQL also handles authorization differently. 
7) Instead of securing entire endpoints like REST, you secure individual fields.
8) A user might see an event's name and date but not the venue data. You can control this at the field level in your schema resolvers.

> [!tip]
> In interviews, mention GraphQL when you see clear over-fetching or under-fetching problems, but don't default to it. Most interviewers appreciate that you know about GraphQL, but they usually prefer to see you solve the core architectural challenges with simpler tools first.

## Related

- [[> API Design]] — back to the API Design section MOC
- [[REST API Design]] — simpler alternative for most use cases
- [[RPC API Design]] — action-oriented alternative for internal service communication
- [[API Security]] — field-level authorization is unique to GraphQL
