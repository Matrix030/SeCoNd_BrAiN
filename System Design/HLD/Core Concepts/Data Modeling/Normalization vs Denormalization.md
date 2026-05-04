---
tags: [system-design, hld, data-modeling]
aliases: ["Normalization", "Denormalization"]
---

# Normalization vs Denormalization

1) Normalization means storing each piece of information in exactly one place.
2) User data lives only in the users table, not duplicated across other tables.
3) This prevents data anomalies where updates happen in one place but not another, leaving your system with inconsistent state.

**Normalized:**

| id  | username | email                                       |
| --- | -------- | ------------------------------------------- |
| 1   | john_doe | [john@example.com](mailto:john@example.com) |
| 2   | jane_doe | [jane@example.com](mailto:jane@example.com) |

| id  | user_id (FK) | content       | created_at          |
| --- | ------------ | ------------- | ------------------- |
| 1   | 1            | Hello, world! | 2024-01-01 10:00:00 |
| 2   | 1            | My first post | 2024-01-01 10:05:00 |

**Denormalized:**

| id  | user_id | username | email                                       | content       | created_at          |
| --- | ------- | -------- | ------------------------------------------- | ------------- | ------------------- |
| 1   | 1       | john_doe | [john@example.com](mailto:john@example.com) | Hello, world! | 2024-01-01 10:00:00 |
| 2   | 1       | john_doe | [john@example.com](mailto:john@example.com) | My first post | 2024-01-01 10:05:00 |

In the denormalized version, if a user changes their username, you'd have to update every single post they've ever made. Miss one update and you have inconsistent data.

In system design interviews, start with a clean normalized model and denormalize only when needed. Avoid repeating data in your schema design. Repeating data is wasteful and creates consistency problems that are much harder to solve than the performance problems you're trying to avoid.

There are a few key exceptions where denormalization might make sense:

- **Analytics and reporting systems** where you're aggregating data that changes infrequently
- **Event logs and audit trails** where you're capturing a snapshot of data at a point in time
- **Heavily read-optimized systems** like search engines where consistency is less critical than speed

That said, even if you need denormalized quick access for performance, you can just put a cache in front that has a denormalized representation of the data. Your source of truth stays clean and normalized, but your cache can have pre-computed joins, aggregations, or whatever structure makes reads fast.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Schema Design Fundamentals]] — consistency requirements inform this decision
- [[Relational Databases]] — normalization is a core SQL concept
- [[Key-Value Stores]] — caches often hold denormalized representations
