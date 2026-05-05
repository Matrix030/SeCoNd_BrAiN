---
tags: [system-design, hld, caching, moc]
aliases: ["Cache", "Caching"]
---

# Caching — Map of Content

> [!info] About this section
> Everything you need to know about caching for system design interviews — where to cache, how to cache, what can go wrong, and how to talk about it.

---

## Where to Cache
- [[Where to Cache]] — external caches, CDNs, client-side, and in-process caching

## Cache Architectures
- [[Cache Architectures]] — cache-aside, write-through, write-behind, and read-through patterns

## Eviction and Expiry
- [[Cache Eviction Policies]] — LRU, LFU, FIFO, and TTL

## Problems and Failures
- [[Common Caching Problems]] — cache stampede, consistency issues, and hot keys

## Interview Strategy
- [[Caching in System Design]] — when to bring up caching and how to walk through it systematically

> [!tip] Default answer
> [[Cache-Aside (Lazy Loading)|Cache-aside]] with [[Redis]] is the default caching pattern for interviews. Start there, then layer on CDN or in-process caching only if the problem calls for it.
