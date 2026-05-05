---
tags: [system-design, hld, scaling]
aliases: ["What is Sharding", "Sharding Definition"]
---

> Your app is taking off. Traffic is growing, users are signing up, and your database keeps getting bigger. At first you solve this by upgrading to a larger database instance with more CPU, memory, and storage. That works for a while.Your app is taking off. Traffic is growing, users are signing up, and your database keeps getting bigger. At first you solve this by upgrading to a larger database instance with more CPU, memory, and storage. That works for a while.

But eventually you hit the ceiling of what a single machine can handle. Queries slow down, writes become a bottleneck, and storage approaches the limit. Even powerful cloud databases like Amazon Aurora max out around 256 TB.

When a single database can't keep up anymore, you have only one real option:

**Split your data across multiple machines.**

This is called **sharding**. While it is a necessity at scale, it also introduces new challenges. We'll cover how and when to shard, as well as what to watch out for.

> [!info]
> People often use the words "partitioning" and "sharding" to mean the same thing. Technically they are slightly different. [[Partitioning]] usually refers to splitting data within a single database instance, often by table ranges or hash partitions. Sharding means splitting data across multiple machines. In practice most engineers use the terms loosely, so do not get hung up on the wording. Just be clear about whether your data lives on one machine or many.

# What is Sharding?

1) Sharding is horizontal [[Partitioning|partitioning]] across multiple machines.
2) Each shard holds a subset of the data, and together the shards make up the full dataset.
3) Unlike partitioning, which stays within a single database instance, sharding spreads the load across many independent databases.

For example, if we partitioned our order data by id, we might end up with something like this:

![[Pasted image 20260505124427.png]]

4) Each shard is a standalone database with its own CPU, memory, storage, and connection pool.
5) No single machine holds all the data or handles all the traffic, which allows both storage capacity and read/write throughput to scale as you add more shards.
6) Sharding solves scaling but introduces new problems.
7) You now have to choose a shard key, route queries to the right shard, avoid hotspots, and rebalance data as shards grow.

## Related

- [[> Sharding]] — back to the Sharding section MOC
- [[Partitioning]] — splitting data within a single database instance
- [[How to Shard Your Data]] — how to choose a shard key and distribution strategy
- [[Sharding Challenges]] — hot spots, cross-shard queries, and consistency trade-offs
