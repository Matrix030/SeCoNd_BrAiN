> Your app is taking off. Traffic is growing, users are signing up, and your database keeps getting bigger. At first you solve this by upgrading to a larger database instance with more CPU, memory, and storage. That works for a while.Your app is taking off. Traffic is growing, users are signing up, and your database keeps getting bigger. At first you solve this by upgrading to a larger database instance with more CPU, memory, and storage. That works for a while.

But eventually you hit the ceiling of what a single machine can handle. Queries slow down, writes become a bottleneck, and storage approaches the limit. Even powerful cloud databases like Amazon Aurora max out around 256 TB.

When a single database can’t keep up anymore, you have only one real option:

**Split your data across multiple machines.**

This is called **sharding**. While it is a necessity at scale, it also introduces new challenges. We'll cover how and when to shard, as well as what to watch out for.

> [!info]
> People often use the words "partitioning" and "sharding" to mean the same thing. Technically they are slightly different. Partitioning usually refers to splitting data within a single database instance, often by table ranges or hash partitions. Sharding means splitting data across multiple machines. In practice most engineers use the terms loosely, so do not get hung up on the wording. Just be clear about whether your data lives on one machine or many.

## First, what is Partitioning?

1) Partitioning means splitting a large table into smaller pieces inside a single database instance. 
2) It does not add more machines. 
3) Instead it organizes data so the database can work more efficiently.
	- Consider an orders table with 500 million rows and 2 TB of data.
	- A query for last month’s orders has to scan the entire table. 
	- Indexes become huge and slow to maintain while routine operations like vacuuming, analyzing, or rebuilding indexes can lock the whole table and impact performance.

4) Partitioning solves this problem by breaking that large table into smaller partitions.
5) The data does not move off the machine.
6) It is simply divided into logical pieces the database can manage separately.
7) Now a query for last month’s orders only scans the relevant partition instead of the full table.

There are two common types of partitioning:

- **Horizontal partitioning**: Split rows across partitions. For example, one partition per year of orders. Same columns, fewer rows per partition.

- **Vertical partitioning**: Split columns across partitions. For example, keep frequently accessed columns in one partition and large or rarely used columns in another. Same rows, fewer columns per partition.

## What is Sharding?

1) Sharding is horizontal partitioning across multiple machines.
2) Each shard holds a subset of the data, and together the shards make up the full dataset.
3) Unlike partitioning, which stays within a single database instance, sharding spreads the load across many independent databases.

For example, if we partitioned our order data by id, we might end up with something like this:

![[Pasted image 20260505124427.png]]

4) Each shard is a standalone database with its own CPU, memory, storage, and connection pool.
5) No single machine holds all the data or handles all the traffic, which allows both storage capacity and read/write throughput to scale as you add more shards.
6) Sharding solves scaling but introduces new problems.
7) You now have to choose a shard key, route queries to the right shard, avoid hotspots, and rebalance data as shards grow.
## How to Shard Your Data

When you decide to shard, you need to make two decisions that work together:

- **What to shard by**: 
	- The field or column you use to split the data.
	- It defines how the data is grouped. 
- **How to distribute it**: 
	- The rule for assigning those groups to shards.
	- It defines how the data is distributed across machines.

### Choosing Your Shard Key

1) A common statement is "I'm going to shard by [field]". The key is knowing what field to use as your shard key and why.

- **Bad shard key** leads to uneven data distribution, hot spots where one shard gets pounded while others sit idle, and queries that have to hit every shard to find what they need. 
- A **good shard key** distributes data evenly, aligns with your query patterns, and scales as your system grows.

1) Here's what makes a good shard key:
	- **High cardinality**: 
		1) The key should have many unique values.
		2) Sharding by a boolean field (true/false) means you can only have two shards max, which defeats the purpose. 
		3) Sharding by user ID when you have millions of users gives you plenty of room to distribute data.

	- **Even distribution**: 
		1) Values should spread evenly across shards.
		2) If you shard by country and 90% of your users are in the US, that shard will be massively larger than the others.
		3) User ID usually distributes well. 
		4) Creation timestamps can work if new records don't all pile onto the most recent shard.
	
	- **Aligns with queries**: 
		1) Your most common queries should ideally hit just one shard.
		2) If you shard users by user_id, queries like "get user profile" or "get user's orders" hit a single shard.
		3) Queries that span all shards become expensive.

	- For example, some good shard keys would be:
		- **user_id** for user-centric app: High cardinality (millions of users), even distribution, and most queries are scoped to a single user anyway ("show me this user's data"). Perfect fit.
		- **order_id** for an e-commerce orders table: High cardinality (millions of orders), queries are usually scoped to a specific order ("get order details", "update order status"), and orders distribute evenly over time.

3) Whereas bad ones could be:

-  **is_premium** (boolean): Only two possible values means only two shards. One shard gets all premium users, the other gets free users. If most users are free, that shard is overloaded.

🔴 **created_at** for a growing table: All new writes go to the most recent shard. That shard becomes a hot spot for writes while older shards handle almost no traffic.

### Sharding Strategies

Once you know your shard key, you need to decide how to distribute that data across shards. There are three main strategies, each with different trade-offs.

#### Range-Based Sharding

Range sharding is the most straightforward. It just groups records by a continuous range of values. You pick a shard key like user_id or created_at, then assign value ranges to shards.

For example, if we were to shard by user_id, we might assign the first 1 million users to shard 1, the next 1 million users to shard 2, and so on.

```
Shard 1 → User IDs 1–1M
Shard 2 → User IDs 1M–2M
Shard 3 → User IDs 2M–3M
```

The main advantage of range-based sharding is simplicity and support for efficient range scans. If you need all orders between user IDs 500K and 600K, you only hit one shard.
