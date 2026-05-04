---
tags: [system-design, hld, data-modeling]
aliases: ["Schema Design", "Start with Requirements"]
---

# Schema Design Fundamentals

Once you've picked your database type, you need to design a schema that supports your system's requirements.

## Start with Requirements

1) Everything flows from three key factors that require careful consideration and were likely already determined during the requirement gathering and api design phases.

- **Data volume** determines where your data can physically live.
	- A social media app with millions of users might need data spread across multiple data stores, which drives schema design choices.
	- If user data and post data need to live on separate systems for performance or organizational reasons, they necessarily need distinct schemas with careful consideration of how they reference each other.

- **Access patterns** are the most important factor and drive most of your design decisions. 
	- How will your data be queried? A news feed that loads "recent posts by followed users" suggests you'll want denormalized data or carefully designed indexes. 
	- An analytics dashboard that aggregates data across time periods might need different table structures entirely. 
	- This comes naturally from your APIs.
	- Just ask what queries will I need to support each endpoint?

- **Consistency requirements** determine how tightly coupled your data can be.
	- Financial transactions need strong consistency (no partial charges), which often means keeping related data in the same database with ACID guarantees.
	- But a user's activity feed can handle eventual consistency (it's okay if a like shows up a few seconds later), which allows you to distribute that data across separate systems with different schemas optimized for different access patterns.

> [!tip]
> In interviews, explicitly tie your schema choices back to these factors. For example, _"Since we need to load feeds quickly and likes can be eventually consistent, I'll denormalize like counts into the posts table."_ That shows you're reasoning instead of memorizing patterns.

All of the schema design techniques that follow ([[Entities Keys and Relationships]], [[Normalization vs Denormalization|normalization]], [[Indexing for Access Patterns|indexes]], [[Scaling and Sharding|sharding]]) are just tools to address these three factors.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Entities Keys and Relationships]] — primary keys, foreign keys, and table relationships
- [[Indexing for Access Patterns]] — indexing strategy tied to your APIs
- [[Normalization vs Denormalization]] — when to keep data clean, when to duplicate
- [[Scaling and Sharding]] — partitioning across machines
