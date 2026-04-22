---
tags: [system-design, hld, api]
aliases: ["API Auth", "API Authentication"]
---

# API Security

Security is often treated as an afterthought, but demonstrating security awareness can set you apart. You don't need to design a bulletproof security system, but showing that you understand basic API security principles signals that you think about production-ready systems.

## Authentication and Authorization

The first question your API needs to answer is: "Who is making this request, and are they allowed to do what they're asking?"

**Authentication** verifies identity - proving the user is who they claim to be.
**Authorization** verifies permissions - checking if that authenticated user is allowed to perform the specific action they're requesting.

For our Ticketmaster example, authentication might verify that the request comes from user "john@example.com", while authorization checks if John is allowed to cancel the specific booking he's trying to cancel (he should only be able to cancel his own bookings, not everyone's).

### API Keys vs JWT Tokens

When designing authentication for your API, you'll typically choose between two main approaches depending on who will be using your API and how they'll access it.

For most (but not all), interviews, authentication and authorization are not a focus. My advice would be to call out which endpoints require the user be authenticated and say that you'd rely on a JWT or store the user's session in a database to authenticate the user.

#### API Keys

1) API keys are long, randomly generated strings that act like passwords for applications rather than humans.
2) When a client makes a request, they include their API key in the Authorization header, and your server looks up that key to identify which application is making the request.

Here's how they work:
- you generate a unique API key for each client (like sk_live_abc123def456...), store it in your database along with any permissions or rate limits for that client, and then verify each incoming request by looking up the key. 
- They're perfect for server-to-server communication where you control both sides.
- When your booking service needs to call your payment service, an API key is straightforward and effective. 
- They also make sense when you're exposing your endpoints to 3rd party developers who need programmatic access to your system.

> [!warning]
> If you're building a user-facing product with user-facing APIs, API keys are almost never the right choice. Users shouldn't be managing long cryptographic strings, and API keys don't expire or carry user context the way user sessions need to.

```
GET /events
Authorization: Bearer sk_live_abc123...
```

#### JWT (JSON Web Tokens)

1) JWT tokens, on the other hand, encode user information directly into the token itself rather than storing session state on your server. 
2) When a user logs in successfully, your server creates a JWT containing their user ID, permissions, and an expiration time, then signs the entire token with a secret key.
3) Conveniently, when that JWT comes back with future requests, you can verify it's authentic by checking the signature, and you can read the user information directly from the token without any database lookups. 
4) The token itself carries all the context you need to authorize the request.
5) JWTs work particularly well for distributed systems because any service with access to the verification key can validate tokens independently.
6) If your mobile app sends a JWT to your API gateway, the gateway can verify the user's identity and forward the request to your booking service with confidence.

```
// JWT payload
{
  "user_id": "123",
  "email": "john@example.com",
  "role": "customer",
  "exp": 1640995200
}
```

7) Use API keys for internal service communication and external developer access.
8) Use JWT tokens for user sessions in web and mobile applications.
9) JWT tokens can be stateless (no database lookup required) and can carry user context, making them ideal for user-facing applications.

### Role-Based Access Control (RBAC)

1) Real systems have different types of users with different permissions.
2) In our Ticketmaster system, customers can book tickets and view their bookings, venue managers can create events and view sales reports, and system admins can access everything.

3) RBAC assigns roles to users and permissions to roles:

```
Roles:
- customer: can book tickets, view own bookings
- venue_manager: can create events, view sales for their venues
- admin: can access everything

User: john@example.com → Role: customer
User: manager@venue.com → Role: venue_manager
```

4) In your API design, you'd check both authentication and authorization:

```
GET /bookings/{id}
1. Is the user authenticated? (valid JWT token)
2. Is the user authorized? (owns this booking OR is admin)
```

At most, in an interview you'd just mention which endpoints can be accessed by which roles, though more times than not this distinction is not relevant.

## Rate Limiting and Throttling

1) Rate limiting prevents abuse by restricting how many requests a client can make in a given time period. 
2) This protects your system from both malicious attacks and accidental overuse.
3) Common strategies include:
	- **Per-user limits**: 1000 requests per hour per authenticated user
	- **Per-IP limits**: 100 requests per hour for unauthenticated requests
	- **Endpoint-specific limits**: 10 booking attempts per minute to prevent ticket scalping
4) You typically implement rate limiting at the API gateway level or using middleware in your application. When limits are exceeded, return a 429 Too Many Requests status code.

> [!info]
> In interviews, mentioning rate limiting shows you understand production concerns, but don't spend time designing the specific algorithms unless asked. A simple "we'll implement rate limiting to prevent abuse" is usually sufficient.

## Related

- [[> API Design]] — back to the API Design section MOC
- [[REST API Design]] — authentication and authorization apply to every REST endpoint
- [[GraphQL API Design]] — GraphQL handles authorization at the field level, not endpoint level
- [[RPC API Design]] — API keys are common for internal RPC service-to-service auth
