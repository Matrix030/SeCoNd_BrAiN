1) API design follows predictable patterns. 
2) You will:
	- pick a protocol
	- define your resources
	- specify how clients pass data and get responses back.

> [!warning]
> Before we go deep here, I want to make one thing super clear: most interviewers don't care deeply about your API design being perfect. They want to see that you can design a reasonable API and move on to the more complex parts of your system.
> 
> That said, if you're interviewing for frontend or product roles, API design matters more since you'll be working closely with APIs daily. Also, for junior roles, there's less expectation on your ability to design distributed systems, so there may be more time spent in the interview on APIs.

## API Types

You'll typically choose between three main API protocols:

1. **REST (Representational State Transfer)**: - 
	- REST uses standard HTTP methods (GET, POST, PUT, DELETE) to manipulate resources identified by URLs. 
	- For standard CRUD operations in web and mobile applications, REST maps naturally to your database operations and HTTP semantics, making it the go-to protocol for most web services. 
	- This should be your default choice.
    
2. **GraphQL**: - 
	- Unlike REST's fixed endpoints, GraphQL uses a single endpoint with a query language that lets clients specify exactly what data they need.
	- Think about a mobile app that needs only basic user information versus a web dashboard that displays comprehensive analytics - with REST, you'd either create multiple endpoints or force clients to fetch more data than they need, but GraphQL lets each client request exactly what it needs in a single query.
	- If your interviewer mentions "flexible data fetching" or talks about avoiding over-fetching and under-fetching, they're signaling you to consider GraphQL.
    
3. **RPC (Remote Procedure Call)**:
	- RPC protocols like gRPC use binary serialization and HTTP/2 for efficient communication between services.
	- While REST treats everything as resources, RPC lets you think in terms of actions and procedures - when your user service needs to quickly validate permissions with your auth service, an RPC call like checkPermission(userId, resource) is more natural than trying to model this as a REST resource.
	- If the interviewer specifically mentions **microservices** or **internal APIs**, consider RPC for those high-performance connections. Use RPC when performance is critical (see [Networking Essentials](https://www.hellointerview.com/learn/system-design/core-concepts/networking-essentials) for deeper protocol details).


> [!tip]
> Default to REST unless you have a specific reason not to. It's well-understood, has great tooling, and works for 90% of use cases. If you're unsure, just say "I'll use REST APIs" and move on.
> 
> For real-time features like notifications, chat, or live updates, you'll need different protocols like WebSockets or Server-Sent Events. These aren't traditional APIs - they're persistent connections.

![[Pasted image 20260420134117.png]]

### REST

#### Resource Modeling

The foundation of good REST API design is identifying your resources correctly. If you've followed the [[Delivery Framework]], resources are just your Core Entities. {TODO - connect this with "core entities" present in "Delivery Framework.md" file}.

Take Ticketmaster as an example. Your core entities might be events, venues, tickets, and bookings. These naturally map to REST resources:

```
GET /events                    # Get all events
GET /events/{id}               # Get a specific event
GET /venues/{id}               # Get a specific venue
GET /events/{id}/tickets       # Get available tickets for an event
POST /events/{id}/bookings     # Create a new booking for an event
GET /bookings/{id}             # Get a specific booking
```

Importantly, REST resources should represent _things_ in your system, not _actions_. Instead of thinking about what users can do (like "book" or "purchase"), think about what exists in your system (events, venues, tickets, bookings).

> [!info]
> Resources should always be plural nouns, i.e., bookings, events, tickets, etc. Most interviewers don't care about this, but some do, and it's easy enough to get right, so you might as well.

When handling relationships between resources, you have two main approaches:
1) You can nest resources when there's a clear parent-child relationship, like /events/{id}/tickets for all tickets belonging to a specific event.
2) You can keep resources flat and use query parameters for filtering, like /tickets?event_id=123.

- The key difference is whether the relationship is required or optional.
- Use path parameters (or nested resources) when the value is required - like /events/{id}/tickets where you always need to specify which event's tickets you want. 
- Use query parameters when the filter is optional - like /tickets?event_id=123&section=VIP where you might want all tickets, or tickets filtered by event, or tickets filtered by both event and section.

> [!tip]
> If the relationship is always required for the query to make sense, use a path parameter. If it's an optional filter among many possible filters, use a query parameter. It is more important to care more about whether you can identify the right resources than perfect URL structure, but showing this understanding demonstrates good API design intuition.

#### HTTP Methods

1) Once you've identified your resources, you need to decide how clients interact with them.
2) HTTP provides a set of methods (verbs) that map naturally to common operations, and understanding when to use each one is crucial for interviews.
	- **GET** is for retrieving data without changing anything.
		- Use GET for /events/{id} to fetch event details or /events to list all events.
	- **POST** creates new resources. 
		- When a user books tickets, you'd POST to /events/{id}/bookings with the booking details in the request body.
		- The server assigns an ID and returns the newly created booking. POST is neither safe nor idempotent. 
		- In other words, calling it multiple times creates multiple bookings.

	- **PUT** replaces an entire resource with what you send, or creates it if it doesn't exist.
		- If you're updating a user's profile completely, PUT to /users/{id} with the full user object.
		- Unlike POST, PUT is idempotent, so sending the same data multiple times results in the same final state.

	- **PATCH** updates part of a resource.
		- When a user changes just their email address, PATCH to /users/{id} with only the email field. 
		- Unlike PUT, PATCH is not guaranteed to be idempotent - the result depends on how you implement it. 
		- A PATCH that says "set email to X" is idempotent, but a PATCH that says "append to list" is not.

	- **DELETE** removes a resource.
		- DELETE /bookings/{id} cancels a booking.
		- It's **idempotent** because repeated calls leave the server in the same state (the resource stays deleted), even if the response codes differ (first call might return 204, subsequent calls might return 404).

3) The key here is **idempotency**, meaning whether repeated requests leave the server in the same state.
4) GET, PUT, and DELETE are idempotent because calling them multiple times doesn't change the final result.
5) POST and PATCH are not guaranteed to be idempotent, which matters when networks fail and clients retry requests.
6) You don't want duplicate bookings from a retry.

#### Passing Data to APIs

1) API endpoints need input to tell the server what to do. 
2) This can be which resources to fetch, what data to update in the database, or how to filter results. 
3) Understanding where to put different types of input is crucial for designing clean, intuitive APIs.

You have **three main options** for passing data to your REST API, and each serves a different purpose.

1) **Path parameters** identify which specific resource you're working with. When you want to get event details, you put the event ID in the path: /events/123. The ID is part of the URL structure itself, making it clear that you're asking for a specific event, not a collection of events. Use path parameters when the value is required to identify the resource - without it, the request doesn't make sense.

**Query parameters** filter, sort, or modify how you retrieve resources. When you want to search for events in a specific city or date range, you use query parameters: /events?city=NYC&date=2024-01-01. These are optional - you could ask for all events without any filters, or apply multiple filters together. Query parameters work well for pagination too: /events?page=2&limit=20. Note that the first option is separated via a ? and all subsequent parameters are separated by &.

**Request body** contains the actual data you're sending to create or update resources. When a user books tickets, you POST to /events/{id}/bookings with the booking details in the request body. Things like how many tickets, seating preferences, and so on. The request body is where you put complex data structures and anything that might be too large or sensitive for a URL.

Each type of input serves a different role in the API's contract. Path parameters are structural, they determine which endpoint you're hitting. Query parameters are modifiers, they change how the endpoint behaves. Request body is payload, it's the data you're actually working with.

Here's a practical example. If you're building a booking system and a user wants to book VIP tickets for a specific event, you'd structure it like this:

```
POST /events/123/bookings?notify=true
{
  "tickets": [
    {"section": "VIP", "quantity": 2},
    {"section": "General", "quantity": 1}
  ],
  "payment_method": "credit_card"
}
```

The event ID (123) is in the path because you need to specify which event you're booking. The notification preference is a query parameter because it's optional behavior. The actual booking details go in the request body because they're the core data you're creating.

#### Returning Data

An API response is made up of two parts:

1. The status code, which indicates whether the request was successful or not.
2. The response body, which contains the data you're returning to the client (typically JSON)

For status codes, stick to the common ones: 200 for success, 201 for created resources, 400 for bad requests, 401 for authentication required, 404 for not found, and 500 for server errors.




