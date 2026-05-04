---
tags: [system-design, hld, data-modeling]
aliases: ["SQL", "Relational Database"]
---

# Relational Databases (SQL)

1) Relational databases organize data into tables with fixed schemas, where rows represent entities and columns represent attributes.
2) They enforce relationships through foreign keys and provide ACID guarantees for transactions.
3) Most system design problems map naturally onto this model.
4) A social media app has users, posts, comments, and likes, all entities with clear relationships. 
5) An e-commerce system has users, products, orders, and payments. 
6) These fit neatly into relational tables where constraints and foreign keys preserve integrity.

**Users table:**

| id (primary key) | username  | email                                       | created_at          |
| ---------------- | --------- | ------------------------------------------- | ------------------- |
| 1                | john_doe  | [john@example.com](mailto:john@example.com) | 2024-01-01 10:00:00 |
| 2                | jane_doe  | [jane@example.com](mailto:jane@example.com) | 2024-01-01 10:05:00 |
| 3                | bob_smith | [bob@example.com](mailto:bob@example.com)   | 2024-01-01 10:10:00 |

**Posts table:**

| id (primary key) | user_id (foreign key) | content       | created_at          |
| ---------------- | --------------------- | ------------- | ------------------- |
| 1                | 1                     | Hello, world! | 2024-01-01 10:00:00 |
| 2                | 1                     | My first post | 2024-01-01 10:05:00 |
| 3                | 2                     | Another post  | 2024-01-01 10:10:00 |

**Likes table:**

| id (primary key) | user_id (foreign key) | post_id (foreign key) | created_at          |
| ---------------- | --------------------- | --------------------- | ------------------- |
| 1                | 1                     | 1                     | 2024-01-01 10:00:00 |
| 2                | 1                     | 2                     | 2024-01-01 10:05:00 |
| 3                | 2                     | 3                     | 2024-01-01 10:10:00 |

7) SQL is great at handling complex queries. 
8) If you need to fetch "all posts by users that a given user follows, ordered by recency," joins make that straightforward.
9) However, be careful with multi-table joins like this - they can become performance traps at scale.
10) Mentioning complex reporting-style queries often raises yellow flags about performance, so you'll want to think through whether they're actually performant enough or if you need denormalized views, caching, or pre-computed results.
11) And when strong consistency is a non-functional requirement, like ensuring payments don't double-charge or inventory doesn't oversell, SQL's ACID guarantees are the right tool for the job.

> [!tip]
> The usual knock on relational databases is scalability, but this is often exaggerated. Modern SQL databases scale with techniques like read replicas, sharding, connection pooling, and caching. Some of the largest companies in the world (Facebook, Airbnb) rely on relational foundations. Scaling isn't just about the database you choose, but how you architect around it.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Database Model Options]] — why SQL is the default
- [[Normalization vs Denormalization]] — keeping SQL schemas clean
- [[Indexing for Access Patterns]] — speeding up SQL queries
- [[Scaling and Sharding]] — scaling relational databases
