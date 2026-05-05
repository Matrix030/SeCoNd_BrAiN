---
tags: [system-design, hld, scaling, moc]
aliases: ["Sharding"]
---

# Sharding — Map of Content

> [!info] About this section
> How and when to split data across multiple machines to scale beyond a single database.

---

## Fundamentals
- [[Partitioning]] — horizontal vs vertical partitioning within a single database instance
- [[What is Sharding]] — what sharding is and how it differs from partitioning

## How to Shard
- [[How to Shard Your Data]] — choosing a shard key and comparing range, hash, and directory strategies

## Challenges
- [[Sharding Challenges]] — hot spots, cross-shard operations, and maintaining consistency

## Reference
- [[Sharding in Modern Databases]] — how Cassandra, DynamoDB, MongoDB, Vitess, and Citus handle sharding
- [[Sharding in System Design]] — when to bring it up, what to say, and what mistakes to avoid in interviews

> [!tip] Interview mental model
> Identify the bottleneck → explain why a single DB won't scale → propose sharding with a justified shard key → call out trade-offs → address resharding.
