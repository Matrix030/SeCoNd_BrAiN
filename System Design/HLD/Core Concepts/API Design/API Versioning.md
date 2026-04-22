---
tags: [system-design, hld, api]
aliases: ["Versioning Strategies", "API Version"]
---

# API Versioning

1) APIs evolve over time, and you need a strategy for handling changes without breaking existing clients. This is particularly important for public APIs where you can't control when clients update their code.
2) The most common approach is **URL versioning**, where you include the version number in the path: /v1/events or /v2/events.
3) This is explicit and easy to understand.
4) Clients know exactly which version they're using just by looking at the URL.
5) It's also simple to implement since you can route different versions to different code paths.
6) **Header versioning** puts the version in an HTTP header instead: Accept-Version: v2 or API-Version: 2.
7) This keeps URLs cleaner and follows HTTP standards better, but it's less obvious to developers and harder to test in browsers.

URL versioning is usually the safer choice because it's more widely understood and easier to explain quickly. Unless you need header versioning, stick with the URL approach.

> [!info]
> You'll see that in breakdowns we don't even include versioning in the API design. This is more a product of it just not being important to most interviewers rather than a statement that it's not important in practice (it is).

## Related

- [[> API Design]] — back to the API Design section MOC
- [[REST API Design]] — versioning is most relevant for REST public APIs
- [[Pagination]] — another common API pattern
