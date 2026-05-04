---
tags: [system-design, hld, database-indexing, database-optimization]
aliases: ["Composite Index", "Multi-Column Index"]
---

## Index Optimization Patterns

1) So far, we've explored the main types of indexes you'll encounter in system design interviews: [[B-Tree Indexes|B-trees]] for general-purpose querying, [[Hash Indexes|hash indexes]] for exact matches, [[Geospatial Indexes|geospatial indexes]] for location data, and [[Inverted Indexes|inverted indexes]] for text search. 
2) Each type solves a specific class of problem, with trade-offs in storage, performance, and flexibility.
3) Experienced engineers spend significant time analyzing their application's read and write patterns, looking for ways to reduce the processing overhead of common queries.
4) They identify performance bottlenecks by examining query plans and database metrics, then strategically improve performance using appropriate indexing strategies.
5) This often requires looking beyond just picking the right type of index - it's about understanding your access patterns and crafting an indexing approach that efficiently supports them.

# Composite Indexes

1) Composite indexes are the most common optimization pattern you'll encounter in practice. 
2) Instead of creating separate indexes for each column, we create a single index that combines multiple columns in a specific order.
3) This matches how we typically query data in real applications.

Consider a typical social media feed query:

```
SELECT * FROM posts 
WHERE user_id = 123 
AND created_at > '2024-01-01'
ORDER BY created_at DESC;
```

We could create two separate indexes:

```
CREATE INDEX idx_user ON posts(user_id);
CREATE INDEX idx_time ON posts(created_at);
```

But this isn't optimal. The database would need to:

1. Use one index to find all posts by user 123
2. Use another index to find all posts after January 1st
3. Intersect these results
4. Sort the final result set by created_at

Instead, a composite index gives us everything we need in one shot:

```
CREATE INDEX idx_user_time ON posts(user_id, created_at);
```

1) When we create a composite index, we're really creating a [[B-Tree Indexes|B-tree]] where each node's key is a concatenation of our indexed columns. 
2) For our (user_id, created_at) index, each key in the B-tree is effectively a tuple of both values. 
3) The B-tree maintains these keys in sorted order based on user_id first, then created_at. Conceptually, the keys might look like:

```
(1, 2024-01-01)
(1, 2024-01-02)
(1, 2024-01-03)
(2, 2024-01-01)
(2, 2024-01-02)
(3, 2024-01-01)
```

4) Now when we execute our query, the database can traverse the B-tree to find the first entry for user_id=123, then scan sequentially through the index entries for that user until it finds entries beyond our date range.
5) Because the entries are already sorted by created_at within each user_id group, we get both our filtering and sorting for free.
6) This structure is particularly efficient because we're using the B-tree's natural ordering to handle multiple conditions in a single index traversal.
7) We've effectively turned our two-dimensional query (user and time) into a one-dimensional scan through ordered index entries.

![[Pasted image 20260504150810.png]]

#### The Order Matters

1) The order of columns in a composite index is crucial.
2) Our index on (user_id, created_at) is great for queries that filter on user_id first, but it's not helpful for queries that only filter on created_at. 
3) This follows from how B-trees work - we can only use the index efficiently for prefixes of our column list.
4) This is why you'll often hear database experts say "order columns from most selective to least selective." But there's more nuance in practice.
5) Sometimes query patterns trump selectivity - if you frequently sort by a particular column, including it in your composite index (even if it's not highly selective) can eliminate expensive sort operations.

Consider common interview scenarios like:

- Order history lookups: (customer_id, order_date)
- Event processing: (status, priority, created_at)
- Activity feeds: (user_id, type, timestamp)

## Related

- [[> Database Indexing]] — back to the section MOC
- [[B-Tree Indexes]] — the underlying structure composite indexes are built on
- [[Covering Indexes]] — the next optimization: include all queried columns in the index
