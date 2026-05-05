---
tags: [system-design, hld, caching]
aliases: ["Caching Interview Strategy", "How to Talk About Caching"]
---

# Caching in System Design

Caching comes up in nearly every system design interview, so it's important to know when to bring it up and how to walk through it systematically.

## When to Bring Up Caching

1) Don't jump straight to caching.
2) You need to establish why it's necessary first.
3) Bring up caching when you identify one of these problems:

Read-heavy workload: "We're serving 10M daily active users, each making 20 requests per day. That's 200M reads hitting the database. Even with indexes, we're looking at 20-50ms per query. A cache drops that to under 2ms and takes most of the load off the database."

Expensive queries: "Computing a user's personalized feed requires joining posts, followers, and likes across multiple tables. That query takes 200ms. We can cache the computed feed for 60 seconds and serve it in 1ms from Redis."

High database CPU: "Our database CPU is hitting 80% during peak hours just serving reads. The same queries run over and over. Caching the hot queries will cut database load by 70-80%."

Latency requirements: "We need sub-10ms response times for the API. Database queries are taking 30-50ms. We have to cache."

4) The pattern is simple.
5) Identify the performance problem, quantify it with rough numbers, and explain how caching solves it.
6) You can use our [Numbers to Know](https://www.hellointerview.com/learn/system-design/core-concepts/numbers-to-know) to get a sense of reasonable database and cache latencies.

## How to Introduce Caching

1) Once you've established the need for caching, walk through your caching strategy systematically:

**1. Identify the bottleneck**

1) Start by pointing to the specific problem caching will solve. Is it database load? Query latency? Expensive computations? Be specific about what's slow and why.

"User profile queries are hitting the database 500 times per second during peak hours. Each query takes 30ms. That's our bottleneck."

**2. Decide what to cache**

1) Not everything should be cached.
2) Focus on data that is read frequently, doesn't change often, and is expensive to fetch or compute.

"We'll cache user profiles since they're read on every page load but only updated when users edit their settings. We'll also cache the trending posts feed since it's computed from expensive aggregations but only needs to refresh every minute."

3) Think about cache keys.
4) How will you look up cached data? For user profiles, the key might be user:123:profile. For trending posts, it could be trending:posts:global.

**3. Choose your cache architecture**

1) Pick a caching pattern that matches your consistency requirements.
2) Write-through makes sense when you need strong consistency.
3) Write-behind works for high-volume writes where you can tolerate some risk.

"I'll use [[Cache-Aside (Lazy Loading)|cache-aside]]. On a read, we check Redis first. If it's there, return it. If not, query the database, store the result in Redis, and return it."

4) If you're dealing with static content like images or videos, mention CDN caching.
5) If you have extremely hot keys that get hammered, mention in-process caching as an optimization layer.

**4. Set an eviction policy**

1) Explain how you'll manage cache size.
2) [[LRU]] is the safe default answer.
3) [[TTL]] is essential for preventing stale data.

"We'll use LRU eviction with Redis and set a TTL of 10 minutes on user profiles. That keeps the cache from growing unbounded while ensuring profiles don't get too stale. If a user updates their profile, we'll invalidate the cache entry immediately."

**5. Address the downsides**

1) Caching introduces complexity.
2) Show you've thought about the trade-offs.

Cache invalidation: How do you keep cached data fresh? Do you invalidate on writes, rely on TTL, or accept eventual consistency?

"When a user updates their profile, we'll delete the cache entry so the next read fetches fresh data from the database."

Cache failures: What happens if Redis goes down? Will your database get crushed by the sudden traffic spike?

"If Redis is unavailable, requests will fall back to the database. We'll add circuit breakers so we don't overwhelm the database with a stampede. We might also consider keeping a small in-process cache as a last-resort layer."

Thundering herd: What happens when a popular cache entry expires and 1000 requests try to refetch it simultaneously?

"For extremely popular keys, we can use probabilistic early expiration or request coalescing so only one request fetches from the database while others wait for that result."

3) Don't list every possible problem.
4) Pick one or two that are relevant to the system you're designing and explain how you'd handle them.
5) For staff-level candidates, focus on the important but non-obvious scenarios rather than burning time on things the interviewer can already assume.

## Conclusion

1) Caching is what you do when reading from the database is too slow or too expensive.
2) It keeps frequently accessed data in fast memory so you can skip the database entirely for most reads.

3) The core trade-off is simple.
4) Caches make reads faster and reduce load on whatever is behind them, but they introduce complexity around staleness and invalidation.
5) Cached data can fall out of sync with the database.
6) Cache failures can crush your database if you're not prepared.
7) [[Hot Keys]] can create bottlenecks even in distributed caches.

8) In interviews, bring up caching after you've identified a database bottleneck.
9) Walk through what you'll cache, which caching pattern you'll use, how you'll handle eviction, and what happens when things go wrong.
10) CDN caching works for static media, and in-process caching can make sense for extremely hot keys.

11) Most importantly, don't cache everything.
12) Show you understand when caching is worth the complexity and when a well-indexed database is enough.

## Related

- [[> Caching]] — back to the Caching MOC
- [[Cache Architectures]] — the four caching patterns
- [[Cache Eviction Policies]] — LRU, LFU, FIFO, and TTL
- [[Common Caching Problems]] — stampede, consistency, and hot keys
