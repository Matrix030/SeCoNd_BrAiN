---
tags: [system-design, hld, database-indexing, moc]
aliases: ["Database Indexing"]
---

# Database Indexing — Map of Content

> [!info] About this section
> Core concepts, index types, and optimization patterns for database indexing in system design.

---

## Foundations
- [[How Database Indexes Work]] — what indexes are, physical storage, access patterns, and cost trade-offs
- [[Wrapping Up]] — key takeaways and interview cheat sheet

## Index Types
- [[B-Tree Indexes]] — the default index type; supports equality and range queries
- [[LSM Trees]] — write-optimized storage engine behind Cassandra, RocksDB, and DynamoDB
- [[Hash Indexes]] — O(1) exact-match lookups; rarely used in disk-based systems
- [[Geospatial Indexes]] — indexing 2D location data with geohash, quadtrees, and R-trees
- [[Inverted Indexes]] — full-text search; powers Elasticsearch and database FTS engines

## Index Optimization Patterns
- [[Composite Indexes]] — multi-column indexes that match real query patterns
- [[Covering Indexes]] — include all queried columns to avoid table lookups

> [!tip] Interview mental model
> Default to B-trees. Switch to LSM trees for write-heavy workloads, geospatial indexes for proximity queries, and inverted indexes for full-text search. Composite indexes are the most common real-world optimization.
