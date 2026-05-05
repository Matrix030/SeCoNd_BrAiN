---
tags: [system-design, hld, caching]
aliases: ["Eviction Policies", "Cache Eviction"]
---

# Cache Eviction Policies

Caches have limited memory, so they need a strategy for deciding which entries to remove when full. These strategies are called eviction policies.

## LRU (Least Recently Used)

- LRU evicts the item that has not been accessed for the longest time.
- It tracks access order using a linked list or ring buffer so the least recently used item can be removed in constant time.
- It is the default in many systems because it adapts well to most workloads where recently used data is likely to be used again.

## LFU (Least Frequently Used)

- LFU evicts the item that has been accessed the least. 
- It maintains a counter for each key and removes the one with the lowest frequency. 
- Some implementations use approximate LFU to avoid the cost of precise frequency tracking.
- This works well when certain keys are consistently popular over time, like trending videos or top playlists.

## FIFO (First In First Out)

- FIFO evicts the oldest item in the cache based only on insertion time.
- It can be implemented with a simple queue, but it ignores usage patterns.
- Because it may evict items that are still hot, it is rarely used in real systems beyond simple caching layers.

## TTL (Time To Live)

- TTL is not an eviction policy by itself. 
- Instead, it sets an expiration time for each key and removes entries that are too old.
- It is often combined with [[LRU]] or [[LFU]] to balance freshness and memory usage.
- TTL is a must have when data must eventually refresh, like API responses or session tokens.

## Related

- [[> Caching]] — back to the Caching MOC
- [[Cache Architectures]] — cache-aside, write-through, write-behind, and read-through
- [[Common Caching Problems]] — cache stampede and hot keys
- [[Caching in System Design]] — how to talk through eviction in interviews
