---
tags: [system-design, hld, scaling, fault-tolerance]
aliases: ["Sharding Challenges", "Hot Spots", "Cross-Shard Operations"]
---

# Challenges of Sharding

1) Sharding solves your scaling problem but introduces new ones.
2) Data is now distributed across multiple machines, which means you have to deal with uneven load, queries that span shards, and maintaining consistency across databases. 
3) These challenges are unavoidable, but you can design around them if you know what to expect.

## Hot Spots and Load Imbalance

1) Even with a good shard key, some shards can end up handling way more traffic than others. 
2) This is called a hot spot, and it negates the main benefit of sharding because one overloaded shard becomes your bottleneck.
3) The most common cause is the celebrity problem. 
4) If you shard users by user_id, Taylor Swift's shard handles 1000x more traffic than a normal user's shard.
5) Every time someone views her profile, likes her post, or sends her a message, that request hits the same shard. 
6) Hash-based distribution doesn't help here because the issue isn't the distribution strategy, it's that some keys are inherently more active than others.
![[Pasted image 20260505140915.png]]

7) Time-based sharding creates a different kind of hot spot.
8) If you shard by creation date, all new writes go to the most recent shard. 
9) That shard handles all the write traffic while older shards sit mostly idle handling only reads of historical data.
10) You can detect hot spots by monitoring shard metrics like query latency, CPU usage, and request volume.
11) When one shard consistently shows higher metrics than others, you have a hot spot problem.

**Here's how to handle them:**
1) **Isolate hot keys to dedicated shards**: 
	- If Taylor Swift's account generates too much traffic, move it to a dedicated shard that only handles celebrity accounts. 
	- This is why directory-based sharding can be useful for specific cases, though you probably wouldn't start there.

2) **Use compound shard keys**: 
- Instead of sharding just by user_id, combine it with another dimension like hash(user_id + date). 
- This spreads a single user's data across multiple shards over time, which helps if the hot spot is both high volume and spans time periods.

1) **Dynamic shard splitting**: 
	- Some databases support automatically splitting a shard when it gets too large or too hot.
	- For example, MongoDB's balancer will split and migrate range-based chunks (including when using a hashed shard key) to maintain balance.
	- By contrast, Vitess supports online resharding, but it is operator-driven (initiated and managed by operators), not automatic.

## Cross-Shard Operations
1) When your data lives on multiple machines, any query that needs data from more than one shard becomes expensive.
2) Instead of querying one database, you have to query multiple shards, wait for all of them to respond, and aggregate the results yourself.
3) The problem shows up with queries that don't align with your shard key.
4) If you shard users by user_id, a query like "get user 12345's profile" hits one shard.
5) Fast and simple. But a query like "get the top 10 most popular posts globally" has to check every shard because posts are scattered across all user shards.
6) You send the query to all 64 shards, wait for all 64 responses, merge the results, and then return the top 10. 
7) That's 64x the network calls and latency.

![[Pasted image 20260505141232.png]]

You can't eliminate cross-shard queries entirely, but you can minimize them:

- **Cache the results**: 
	- If "top 10 most popular posts" requires hitting all shards, cache the result for 5 minutes.
	- The first query is expensive, but the next thousand requests hit the [[> Caching|cache]] instead of querying 64 shards.
	- This works especially well for queries that don't need real-time accuracy (ie. eventual consistency is acceptable). 
	- Leaderboards, trending content, and aggregate stats are perfect candidates.

- **Denormalize to keep related data together**: 
	- If you frequently need to query posts along with user data, store some post information directly on the user's shard. 
	- Yes, this duplicates data. Yes, it makes updates more complex. But it lets you query everything from one shard, which is often worth the trade-off.

- **Accept the hit for rare queries**: 
	- Sometimes a query genuinely needs to hit all shards and that's okay as long as it's infrequent. 
	- An admin dashboard that shows "total users across all shards" can afford to be slow if it's only loaded a few times a day.

> [!tip]
> Cross-shard operations are often a signal that something in your design needs rethinking. If you find yourself saying "we'll query all shards and aggregate the results" for a common use case, pause and consider: Can I denormalize to avoid this? Can I cache it? Can I precompute it with a background job? Interviewers expect you to minimize cross-shard queries, not just accept them as inevitable.

## Maintaining Consistency

1) When your data lives on a single database, transactions are straightforward.
2) If you need to deduct inventory and create an order record atomically, you wrap both operations in a database transaction.
3) Either both succeed or both fail.
4) The database handles the consistency guarantees.

5) **Sharding breaks this.**
	- If the user's account lives on shard 1 and the transaction record lives on shard 2, you can't use a single database transaction anymore.
	- You're coordinating writes across two independent databases that don't know about each other.

6) The textbook solution is [two-phase commit (2PC)](https://www.hellointerview.com/learn/system-design/patterns/dealing-with-contention#two-phase-commit-2pc), where a coordinator asks all shards to prepare the transaction, waits for everyone to confirm they're ready, then tells everyone to commit.
7) This guarantees consistency but is slow and fragile.
8) If any shard or the coordinator fails mid-transaction, the whole system can get stuck.
9) Most production systems avoid 2PC because the performance and reliability trade-offs aren't worth it.


**So what do you do instead?**

1) **Design to avoid cross-shard transactions**: 
	- This is the best solution. 
	- If you shard users by user_id, keep all of a user's data on their shard.
	- Account balance, transaction history, profile information, all on one shard. 
	- Now all your transactions are single-shard transactions, which are fast and reliable.

2) **Use [sagas](https://www.hellointerview.com/learn/system-design/patterns/dealing-with-contention#saga-pattern) for multi-shard operations**: 
	- When you absolutely need to coordinate across shards, use the [saga pattern](https://www.hellointerview.com/learn/system-design/patterns/dealing-with-contention#saga-pattern). 
	- Break the operation into a sequence of independent steps, each with a compensating action.
	- If step 3 fails, you run compensating actions for steps 2 and 1 to undo the work.
	- This gives you eventual consistency without the fragility of 2PC.

	- For example, transferring money between users on different shards:
		1. Deduct money from User A's account (shard 1)
		2. Add money to User B's account (shard 2)
		3. If step 2 fails, refund User A (compensating action)

3) **Accept eventual consistency**: 
	- For many operations, strict consistency isn't required.
	- If you're updating a user's follower count and that count is denormalized across multiple shards for fast profile lookups, it's fine if some shards show different counts for a few seconds.
	- Eventually all shards will converge to the correct number. 
	- This is much simpler than coordinating a distributed transaction, and for most applications, a brief period of inconsistency is acceptable.

The TLDR is that most applications can be designed to avoid cross-shard transactions entirely. If you find yourself constantly needing distributed transactions, you probably chose the wrong shard key or the wrong shard boundaries.

## Related

- [[> Sharding]] — back to the Sharding section MOC
- [[How to Shard Your Data]] — how to choose a shard key to minimize these challenges
- [[> Caching]] — caching cross-shard query results to avoid repeated fan-out
- [[Sharding in System Design]] — how to discuss these trade-offs in an interview
