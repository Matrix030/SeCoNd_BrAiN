---
tags: [system-design, hld, networking, http, api]
aliases: ["RESTful API", "REST API", "Representational State Transfer"]
---

# REST: Simple and Flexible

1) While HTTP can be used directly to build _websites_, oftentimes system designs are concerned with the communication between _services_ via APIs.
2) For creating these APIs, we have three main paradigms: REST, GraphQL, and gRPC.
3) It's a simple and flexible way to create APIs that are easy to understand and use.
4) The **core principle** behind REST is that clients are often performing simple operations against **resources** (think of them like database tables or files on a server).
5) In RESTful API design, the primary challenge is to model your resources and the operations you can perform on them.
6) RESTful API's take advantage of the HTTP methods or verbs together with some opinionated conventions about the paths and the body of the request.
7) They often use JSON to represent the resources in both the request and response bodies — although it's not strictly required.


A simple RESTful API might look like this (where User is a JSON object representing a user):

```
GET /users/{id} -> User
```

Here we're using the HTTP method "GET" to indicate that we're requesting a resource. The {id} is a placeholder for the resource ID, in this case the user ID of the user we want to retrieve.

When we want to update that user, we can use the HTTP method "PUT" to indicate that we're updating a pre-existing resource.

```
PUT /users/{id} -> User
{
  "username": "john.doe",
  "email": "john.doe@example.com"
}
```

We can also create new resources by using the HTTP method "POST". We'll include the body the content of the resource we want to create. Note that I'm not specifying an ID here because the server will assign one.

```
POST /users -> User
{
  "username": "stefan.mai",
  "email": "stefan@hellointerview.com"
}
```

Finally, resources can be nested to represent relationships between resources. For example, a user might have many posts, so we can represent that relationship by nesting the posts under the user resource.

```
GET /users/{id}/posts -> [Post]
```

> [!warning]
> Many engineers often think in terms of methods like updateUser or startGame. These are operations, not resources, so they're not RESTful.
>
> In REST, we want to think in terms of resources and the operations you can perform on them. So our updateUser might be PUT /users/{id} and our startGame might be PATCH /games with { "status": "started" }.

##### Where to Use It

Overall REST is very flexible for a wide variety of use-cases and applications. [ElasticSearch](https://www.hellointerview.com/learn/system-design/deep-dives/elasticsearch) uses it to manage documents, configure indexes, and more.

REST is [not going to be the most performant solution](https://medium.com/@i.gorton/scaling-up-rest-versus-grpc-benchmark-tests-551f73ed88d4) for very high throughput services, and generally speaking JSON is a pretty inefficient format for serializing and deserializing data.

That said, most applications aren't going to be bottlenecked by request serialization. Like TCP. It's well-understood and a good baseline for building scalable systems. You should reach for GraphQL, gRPC, SSE, or WebSockets if you have specific needs that REST can't meet. For practical REST API design patterns, see our [API Design](https://www.hellointerview.com/learn/system-design/core-concepts/api-design) guide.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[HTTP and HTTPS]] — REST is built on top of HTTP
- [[GraphQL]] — when REST under/over-fetching becomes a problem
- [[gRPC]] — when REST performance isn't sufficient for internal services
