---
tags: [system-design, hld, api, moc]
aliases: ["API Design"]
---

# API Design — Map of Content

> [!info] About this section
> Covers the three main API protocols, common patterns, and security considerations for system design interviews.

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

1. **[[REST API Design|REST (Representational State Transfer)]]**: - 
	- REST uses standard HTTP methods (GET, POST, PUT, DELETE) to manipulate resources identified by URLs. 
	- For standard CRUD operations in web and mobile applications, REST maps naturally to your database operations and HTTP semantics, making it the go-to protocol for most web services. 
	- This should be your default choice.
    
2. **[[GraphQL API Design|GraphQL]]**: - 
	- Unlike REST's fixed endpoints, GraphQL uses a single endpoint with a query language that lets clients specify exactly what data they need.
	- Think about a mobile app that needs only basic user information versus a web dashboard that displays comprehensive analytics - with REST, you'd either create multiple endpoints or force clients to fetch more data than they need, but GraphQL lets each client request exactly what it needs in a single query.
	- If your interviewer mentions "flexible data fetching" or talks about avoiding over-fetching and under-fetching, they're signaling you to consider GraphQL.
    
3. **[[RPC API Design|RPC (Remote Procedure Call)]]**:
	- RPC protocols like [[gRPC]] use binary serialization and HTTP/2 for efficient communication between services.
	- While REST treats everything as resources, RPC lets you think in terms of actions and procedures - when your user service needs to quickly validate permissions with your auth service, an RPC call like checkPermission(userId, resource) is more natural than trying to model this as a REST resource.
	- If the interviewer specifically mentions **microservices** or **internal APIs**, consider RPC for those high-performance connections. Use RPC when performance is critical (see [[Networking Essentials]] for deeper protocol details).


> [!tip]
> Default to REST unless you have a specific reason not to. It's well-understood, has great tooling, and works for 90% of use cases. If you're unsure, just say "I'll use REST APIs" and move on.
> 
> For real-time features like notifications, chat, or live updates, you'll need different protocols like [[WebSockets]] or [[Server-Sent Events]]. These aren't traditional APIs - they're persistent connections.

![[Pasted image 20260420134117.png]]

---

## Protocols
- [[REST API Design]] — resource-oriented HTTP design; the default choice for most systems
- [[GraphQL API Design]] — flexible query language for diverse client data needs
- [[RPC API Design]] — action-oriented communication for high-performance service-to-service calls

## Patterns
- [[Pagination]] — offset-based and cursor-based strategies for large datasets
- [[API Versioning]] — URL and header strategies for backward-compatible API evolution

## Security
- [[API Security]] — authentication, authorization, RBAC, and rate limiting

---

Focus on choosing the right protocol for your use case (usually [[REST API Design|REST]]), modeling your resources clearly, and showing you understand the basics of authentication and security.

It's all about balance in an interview. Spend enough time to show you can design a reasonable API, but don't get bogged down in details when there are bigger architectural challenges to tackle. Your interviewer wants to see that you can build systems that work, not that you've memorized every HTTP status code. In practice, candidates mess up more often by spending too much time on API design than by underinvesting. Do your best to not spend more than 5 minutes outlining your APIs in the interview.
