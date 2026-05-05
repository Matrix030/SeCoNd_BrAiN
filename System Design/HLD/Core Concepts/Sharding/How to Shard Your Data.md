---
tags: [system-design, hld, scaling]
aliases: ["How to Shard", "Shard Key", "Sharding Strategies", "Choosing Your Shard Key"]
---

# How to Shard Your Data

When you decide to shard, you need to make two decisions that work together:

- **What to shard by**: 
	- The field or column you use to split the data.
	- It defines how the data is grouped. 
- **How to distribute it**: 
	- The rule for assigning those groups to shards.
	- It defines how the data is distributed across machines.

## Choosing Your Shard Key

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

- **is_premium** (boolean): Only two possible values means only two shards. One shard gets all premium users, the other gets free users.
- If most users are free, that shard is overloaded.
- **created_at** for a growing table: All new writes go to the most recent shard. That shard becomes a hot spot for writes while older shards handle almost no traffic.

## Sharding Strategies

Once you know your shard key, you need to decide how to distribute that data across shards. There are three main strategies, each with different trade-offs.

### Range-Based Sharding
1) Range sharding is the most straightforward.
2) It just groups records by a continuous range of values.
3) You pick a shard key like user_id or created_at, then assign value ranges to shards.
4) For example, if we were to shard by user_id, we might assign the first 1 million users to shard 1, the next 1 million users to shard 2, and so on.

```
Shard 1 → User IDs 1–1M
Shard 2 → User IDs 1M–2M
Shard 3 → User IDs 2M–3M
```

5) The main advantage of range-based sharding is simplicity and support for efficient range scans.
6) If you need all orders between user IDs 500K and 600K, you only hit one shard.

> [!warning]
> Most real-world access patterns don't distribute evenly across ranges. If you shard orders by created_at, almost all your traffic hits the most recent shard because users care about recent orders. New writes only go to the latest shard. Old shards sit mostly idle.

7) Range-based sharding works best when different users naturally query different ranges.
8) Multi-tenant systems, for example, are a good fit.
9) These are systems where each company gets a range of IDs.
10) Think of a SaaS application where each client has a range of user IDs.
11) Company A's users only query Company A's range, and Company B's users only query Company B's range.
12) This distributes the load across shards.

### Hash-Based Sharding (Default)

1) Hash sharding uses a hash function to evenly distribute records across shards.
2) Instead of assigning ranges, you take a shard key like user_id, hash it, and use the result to pick a shard.

For example, if we had 4 shards, we could route users like this:

```
shard = hash(user_id) % 4

User 42  → hash(42) % 4 = Shard 2
User 99  → hash(99) % 4 = Shard 3
User 123 → hash(123) % 4 = Shard 1
```

3) The big advantage of hash-based sharding is even distribution.
4) Since the hash function scrambles the input values, new users get distributed evenly across all shards.
5) The downside shows up when you need to add or remove shards.
6) If you go from 4 shards to 5, the modulo operation changes from % 4 to % 5, which means almost every record maps to a different shard.
7) You have to move massive amounts of data around.
8) This is where **[[Consistent Hashing]]** comes in.
9) Instead of simple modulo, [[Consistent Hashing|consistent hashing]] minimizes data movement when you add or remove shards.
10) We cover this in detail in our [consistent hashing page](https://www.hellointerview.com/learn/system-design/core-concepts/consistent-hashing), but the key point is that hash-based sharding works great as long as you have a plan for resharding.

> [!info]
> Generally speaking, this is the default and most common sharding strategy. It's also what your interviewer will likely assume you're using unless you explicitly state otherwise.

### Directory-Based Sharding

1) Directory sharding uses a lookup table to decide where each record lives.
2) Instead of using a formula, you store shard assignments in a mapping table or service.

For example:

```
user_to_shard
---------------
User 15   → Shard 1
User 87   → Shard 4
User 204  → Shard 2
```

3) The power of directory-based sharding is flexibility.
4) If a particular user generates tons of traffic, you can move them to a dedicated shard.
5) If you need to rebalance load, you just update the mapping table.
6) You can implement complex sharding logic that would be impossible with a simple hash function.
7) The downside is that every single request requires a lookup. 
8) Before you can query user data, you have to ask the directory service which shard that user lives on. 
9) This adds latency to every request and makes the directory service a critical dependency.
10) If the directory goes down, your entire system stops working even if all the data shards are healthy.
11) Directory-based sharding makes sense when you need maximum flexibility and can afford the extra lookup cost. 
12) Most systems start with hash-based or range-based sharding and only use a directory if they have specific requirements that demand it.

> [!warning]
> Realistically, while directory-based sharding is a valid solution for dynamic use cases, it is rarely the answer in a system design interview. It introduces a single point of failure and adds latency to every request, which will prompt your interviewer to ask a number of follow-up questions that could derail the conversation.

## Related

- [[> Sharding]] — back to the Sharding section MOC
- [[What is Sharding]] — what sharding is and how it differs from partitioning
- [[Consistent Hashing]] — the technique that makes hash-based sharding resilient to resharding
- [[Sharding Challenges]] — hot spots, cross-shard queries, and consistency trade-offs
