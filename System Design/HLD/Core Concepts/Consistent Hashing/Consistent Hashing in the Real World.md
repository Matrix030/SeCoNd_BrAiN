---
tags: [system-design, hld, scaling]
aliases: ["Consistent Hashing Real World"]
---

# Consistent Hashing in the Real World

While our example focused on scaling a database, note that consistent hashing applies to any scenarios where you need to distribute data across a cluster of servers. This cluster could be databases, sure, but they could also be caches, message brokers, or even just a set of application servers.

We see consistent hashing (or variations of it) used in many heavily relied on, scaled, systems. For example:

1. [Apache Cassandra](https://www.hellointerview.com/learn/system-design/deep-dives/cassandra): Uses consistent hashing to distribute data across the ring
2. [Amazon's DynamoDB](https://www.hellointerview.com/learn/system-design/deep-dives/dynamodb): Uses consistent hashing under the hood for partition placement
3. [Content Delivery Networks (CDNs)](https://en.wikipedia.org/wiki/Content_delivery_network): Use consistent hashing to determine which edge server should cache specific content

> [!info]
> Not every distributed system uses consistent hashing. Redis Cluster, for example, uses a fixed hash slot approach instead. It divides the key space into 16,384 slots using CRC16(key) mod 16384 and assigns ranges of slots to nodes. This is simpler to reason about, though it requires more coordination when rebalancing. The choice between consistent hashing and fixed hash slots is a real design trade-off you might discuss in an interview.

## Related

- [[> Consistent Hashing]] — back to the section MOC
- [[When to Use Consistent Hashing]] — interview guidance on when to reach for this concept
- [[Sharding in Modern Databases]] — how specific databases handle data distribution
