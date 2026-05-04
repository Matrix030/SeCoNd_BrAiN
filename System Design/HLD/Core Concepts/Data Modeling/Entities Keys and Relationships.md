---
tags: [system-design, hld, data-modeling]
aliases: ["Primary Keys", "Foreign Keys", "Entities and Relationships"]
---

# Entities, Keys & Relationships

1) Once you've identified your core entities, the next step is to map them into tables (or collections) with clear identifiers and relationships.
2) For a social media app, you might have users, posts, comments, and likes. Each entity needs a **primary key** to identify individual records. Use system-generated IDs like user_id or post_id rather than business data like email addresses. System-generated keys stay stable even when business rules change.

```
users: id (PK), username, email
posts: id (PK), user_id (FK → users.id), content, created_at
comments: id (PK), post_id (FK → posts.id), user_id (FK → users.id), content
likes: user_id (FK → users.id), post_id (FK → posts.id)
```

3) This shows the core relationships:
- each post belongs to one user (posts.user_id), each comment belongs to one post and one user, and likes connect users to posts.
- The (PK) marks primary keys, (FK) marks foreign keys with arrows showing what they reference.

> [!tip]
> In interviews, just pick an obvious primary key and explain why. "post_id will be our primary key so we can uniquely identify each post and reference it from comments and likes."

4) With entities defined, connect them with relationships:

- **One-to-many (1:N):** a user has many posts, a post has many comments.
- **Many-to-many (N:M):** users like many posts, posts are liked by many users.
- **One-to-one (1:1):** rare in practice, often a sign that two tables should just be merged.

4) These relationships are enforced through **foreign keys** in SQL (e.g., posts.user_id → users.id) or by application logic in NoSQL. 
5) Foreign keys help ensure referential integrity - meaning they prevent orphaned records like a post referencing a user that doesn't exist, or comments pointing to deleted posts. 
6) However, they come at a cost because the database has to validate each insert/update. 
7) At very large scale, some companies drop them for write performance and enforce integrity at the application level.
8) In an interview, mentioning them shows you understand the trade-off.
9) Finally, layer in **constraints** like NOT NULL, UNIQUE, or CHECK.
10) These enforce correctness at the database level (emails must be unique, prices must be positive). 
11) They protect data quality, though they also add write overhead.

> [!tip]
> Keep your schema grounded in the problem domain: users, tweets, follows if you're modeling Twitter, not abstract "entities" and "relationships." Then show how keys, foreign keys, and constraints keep that model correct and scalable.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Schema Design Fundamentals]] — how requirements drive these design choices
- [[Normalization vs Denormalization]] — how to structure your tables once entities are defined
- [[Indexing for Access Patterns]] — what to index after your schema is set
