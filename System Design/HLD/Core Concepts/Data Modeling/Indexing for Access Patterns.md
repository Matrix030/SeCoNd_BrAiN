---
tags: [system-design, hld, data-modeling]
aliases: ["Database Indexes", "Query Indexing"]
---

# Indexing for Access Patterns

1) Indexes are data structures that help the database find records quickly without scanning every row.
2) Think of them like the index in a book - instead of reading every page to find "normalization," you look it up in the index and jump directly to page 149.
3) While data modeling in an interview, you'll typically want to callout which columns are indexed and why.
4) Your indexes should directly support your most important queries. For a social media app:
	- Index on posts.user_id to quickly find all posts by a user
	- Index on posts.created_at to load recent posts chronologically
	- Composite index on (user_id, created_at) to efficiently load a user's recent posts

> [!tip]
> In interviews, connect your indexes directly to your API endpoints. "The GET /users/{id}/posts endpoint needs an index on posts.user_id" shows you're thinking about real query performance.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Schema Design Fundamentals]] — access patterns are one of three factors that drive schema design
- [[Entities Keys and Relationships]] — indexes build on top of your table definitions
- [[Scaling and Sharding]] — indexes and shard keys interact
