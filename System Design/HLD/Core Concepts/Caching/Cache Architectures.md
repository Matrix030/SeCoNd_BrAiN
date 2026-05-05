---
tags: [system-design, hld, caching]
aliases: ["Cache Patterns", "Caching Patterns"]
---

# Cache Architectures

Not all caching works the same way. How you read from and write to the cache changes performance, consistency, and complexity. These are the four core cache patterns you should know for system design interviews.

## Cache-Aside (Lazy Loading)

This is the most common caching pattern and the one you should default to in interviews.

How it works:

1. Application checks the cache.
2. If the data is there, return it.
3. If not, fetch from the database, store it in the cache, and return it.

![[Pasted image 20260504170914.png]]

Cache-aside only caches data when needed, which keeps the cache lean. The downside is that a cache miss causes extra latency.

> [!tip]
> If you only remember one caching pattern for interviews, make it cache-aside.

## Write-Through Caching

1) With write-through caching, the application writes only to the cache.
2) The cache then synchronously writes to the database before returning to the application. 
3) The write operation does not complete until both the cache and database are updated.
4) In practice, this requires a cache implementation that supports write-through, like a caching library with a data store plugin. 
5) When you write to the cache, the library handles calling your database write logic before acknowledging the write. 
6) [[Redis]] itself does not natively support write-through, so you need application code or a framework to implement this pattern.

![[Pasted image 20260504171025.png]]

7) The tradeoff is slower writes because the application must wait for both the cache update and the database write to complete.
8) Write-through can also pollute the cache with data that may never be read again.
9) Write-through still suffers from the dual-write problem.
10) If the cache update succeeds but the database write fails, or vice versa, the systems can end up inconsistent.
11) You need retry logic, error handling, or eventually accept that perfect consistency is difficult without distributed transactions.
12) In system design interviews, write-through is less common than cache-aside because it requires specialized caching infrastructure and still has consistency edge cases.
13) Use this when **reads must always return fresh data** and your system can tolerate slightly slower writes.

## Write-Behind (Write-Back) Caching

1) With write-behind caching, the application writes only to the cache. 
2) The cache batches and writes the data to the database asynchronously in the background.

![[Pasted image 20260504171506.png]]

3) This makes writes very fast, but introduces risk.
4) If the cache crashes before flushing, you can lose data.
5) This is best for workloads where occasional data loss is acceptable.
6) Use this when **you need high write throughput** and **eventual consistency is acceptable**. Common in analytics and metrics pipelines.

## Read-Through Caching

1) With read-through caching, the cache acts as a smart proxy. 
2) Your application never talks to the database directly.
3) On a cache miss, the cache itself fetches from the database, stores the data, and returns it.
4) This is the read equivalent of write-through. 
5) In both patterns, the cache acts as an intermediary that handles database operations.
6) Read-through manages reads, write-through manages writes. Systems often combine both patterns.

![[Pasted image 20260504171823.png]]

7) This centralizes caching logic but adds complexity and usually requires a specialized caching library or service.
8) It is less common in practice than cache-aside.
9) CDNs are a form of read-through cache.
10) When a CDN gets a cache miss, it fetches from your origin server, caches the result, and returns it. 
11) But for application-level caching with Redis, cache-aside is far more common.
12) Generally speaking, there are very few reasons to propose this pattern in system design interviews unless you're discussing CDNs or similar infrastructure.

## Related

- [[> Caching]] — back to the Caching MOC
- [[Where to Cache]] — external caches, CDNs, client-side, and in-process caching
- [[Cache Eviction Policies]] — LRU, LFU, FIFO, and TTL
- [[Common Caching Problems]] — consistency issues and failure modes
