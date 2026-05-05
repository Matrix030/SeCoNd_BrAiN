---
tags: [system-design, hld, scaling]
aliases: ["Sharding in System Design", "Sharding Interview"]
---

# Sharding in System Design 

Sharding comes up just about anytime you are discussing scaling. The key is knowing when to bring it up, what to say, and what mistakes to avoid.

## When to Mention Sharding

1) Be careful not to make the mistake of prematurely sharding. You need to establish why a single database won't work first.
2) Bring up sharding when you're discussing capacity planning and hit one of these limits:
	- **Storage**: "We have 500M users with 5KB of data each, that's 2.5TB. A single Postgres instance can handle that, but if we grow 10x we'll need to shard."
	- **Write throughput**: "We're expecting 50K writes per second during peak. A single database will struggle with that write load, so we should shard."
	- **Read throughput**: "Even with read replicas, if we're serving 100M daily active users making multiple queries each, we'll need to distribute the read load across shards."

The formula is simple:
1. Identify the bottleneck
2. Explain why single database won't scale
3. Propose sharding

You can use our [Numbers to Know](https://www.hellointerview.com/learn/system-design/core-concepts/numbers-to-know) in order to get a good sense of when you may hit reasonable limits with a single database.

> [!warning]
> By far the number one sharding mistake I see in interviews is candidates introducing sharding before they've proven it's necessary. Slow down, do the math, and make sure sharding is actually needed before you start explaining how you'd do it.

## What to Say

Here's how to walk through sharding in an interview using a social media app as an example:

**1. Propose a shard key based on your access patterns** 
- "For this social media app, most queries are user-centric.
- When someone loads their feed, we're querying their posts, their followers, their likes.
- That's all scoped to a single user. So I'd shard by user_id."

**2. Choose your distribution strategy** 
- "I'd use hash-based sharding with [[Consistent Hashing|consistent hashing]]. 
- Hash the user_id to distribute users evenly across shards."

**3. Call out the trade-offs** 
- "The trade-off is that global queries become expensive. 
- If we need 'trending posts across all users' we have to query all shards and aggregate results. 
- We can handle that by caching trending content and pre-computing it with a background job rather than calculating it on every request."

**4. Address how you'll handle growth** 
- "We'll start with 64 shards, which gives us room to grow.
- [[Consistent Hashing|Consistent hashing]] makes it easier to add shards later without resharding all the data.
- If we need more capacity, we can add shards and only a fraction of the data moves."

Notice how this flows naturally. You're not just listing facts, you're walking through your reasoning and showing you understand the trade-offs.

## Conclusion

Sharding is what you do when a single database can't handle your scale anymore. You split data across multiple machines to increase storage capacity and throughput.

There are two main decisions that matter: pick a shard key that aligns with your query patterns, and choose a distribution strategy that spreads load evenly. Get these wrong and you'll have hot spots and expensive cross-shard queries.

Bring up sharding when you've identified a database bottleneck. Walk through your shard key choice, explain the trade-offs, and show you've thought about cross-shard queries and resharding. Most importantly, don't shard too early. A well-tuned single database can get you surprisingly far.

## Related

- [[> Sharding]] — back to the Sharding section MOC
- [[How to Shard Your Data]] — shard key selection and distribution strategies
- [[Sharding Challenges]] — trade-offs to call out during the interview
- [[Sharding in Modern Databases]] — which databases to mention and how
