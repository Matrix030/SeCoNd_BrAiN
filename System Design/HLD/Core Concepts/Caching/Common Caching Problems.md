---
tags: [system-design, hld, caching, fault-tolerance]
aliases: ["Cache Problems", "Cache Failures", "Caching Pitfalls"]
---

# Common Caching Problems

- Caching makes systems faster, but it also introduces new failure modes.
- These problems show up in real systems at scale, and interviewers often use them to test whether you understand the trade-offs of caching, not just the benefits. 
- If you bring up caching in an interview, you should also show that you can handle these edge cases.

## Cache Stampede (Thundering Herd)

- A cache stampede happens when a popular cache entry expires and many requests try to rebuild it at the same time.
- There is a brief window, even if only a second, where every request misses the cache and goes straight to the database.
- Instead of one query, you suddenly have hundreds or thousands, which can overload the database.

![[Pasted image 20260504172206.png]]

- For example, imagine your system caches the homepage feed with a [[TTL]] of 60 seconds.
- When the cache expires at exactly 12:01:00, every request at that moment misses the cache and queries the database. 
- If traffic is high, this spike can overwhelm the database and cause cascading failures.

- How to handle it:
	- **Request coalescing (single flight):** Allow only one request to rebuild the cache while others wait for the result. This is the most effective solution.
	- **Cache warming:** Refresh popular keys proactively before they expire. This only helps when using TTL-based expiration. If you invalidate cache on writes instead, warming does not prevent stampedes.

## Cache Consistency

1) Cache consistency problems are some of the most commonly discussed in system design interviews.
2) They happen when the cache and database return different values for the same data.
3) This is common because most systems read from the cache but write to the database first.
4) That creates a window where the cache still holds stale data.
5) For example, if a user updates their profile picture, the new value is written to the database but the old value might still be in the cache.
6) Other users may see the outdated profile picture until the cache eventually refreshes.

There is no perfect solution. You choose a strategy based on how fresh the data must be.

How to handle it:

- **Cache invalidation on writes:** Delete the cache entry after updating the database so it gets repopulated with fresh data.
- **Short TTLs for stale tolerance:** Let slightly stale data live temporarily if eventual consistency is acceptable.
- **Accept eventual consistency:** For feeds, metrics, and analytics, a short delay is usually fine.

## Hot Keys

1) A hot key is a cache entry that receives a huge amount of traffic compared to everything else.
2) Even if the cache hit rate is high, a single hot key can overload one cache node or one [[Redis]] shard and become a bottleneck.
3) For example, if you are building Twitter and everyone is viewing Taylor Swift's profile, the cache key for her user data (user:taylorswift) may receive millions of requests per second. 
4) That one key can overload a single Redis node even though everything is working "correctly."

How to handle it:

- **Replicate hot keys:** Store the same value on multiple cache nodes and load balance reads across them.
- **Add a local fallback cache:** Keep extremely hot values in-process to avoid pounding Redis.
- **Apply rate limiting:** Slow down abusive traffic patterns on specific keys.

## Related

- [[> Caching]] — back to the Caching MOC
- [[Cache Architectures]] — how caching patterns affect consistency
- [[Cache Eviction Policies]] — TTL and its role in stampedes
- [[Caching in System Design]] — how to address these problems in interviews
