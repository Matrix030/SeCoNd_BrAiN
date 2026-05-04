---
tags: [system-design, hld, data-modeling]
aliases: ["Sharding", "Database Partitioning"]
---

# Scaling and Sharding

1) When your data gets too large for a single database, you need to shard it across multiple machines. 
2) The key is choosing a partition strategy that keeps related data together.
3) **Shard by the primary access pattern**:
	- If you mostly query "posts by user," shard by user_id.
	- This keeps a user's posts on the same database, avoiding expensive cross-shard queries.

> [!warning]
> Be careful with time-range sharding. While it sounds appealing for "recent posts" queries, all current writes hit the same shard (the latest time range), creating a hot shard. This is usually an anti-pattern for write-heavy systems. Time-range partitioning works better for archival or analytics workloads where recent data is read-heavy but writes are spread out.

4) **Avoid cross-shard queries** whenever possible. 
5) If your timeline feature needs to show posts from multiple users a user follows, and you've sharded by user_id, you'll have to query multiple shards and merge results. This is expensive and complex.
![[Pasted image 20260424144428.png]]

> [!warning]
> Your choice of shard key is often permanent and affects every query. Think carefully about your primary access patterns before choosing how to shard your data.

At the end, your whiteboard should look something like this:
![[Pasted image 20260424144454.png]]

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Schema Design Fundamentals]] — data volume is one of three factors driving schema design
- [[Indexing for Access Patterns]] — indexes and shard keys interact
- [[Wide-Column Databases]] — built for horizontal scale
