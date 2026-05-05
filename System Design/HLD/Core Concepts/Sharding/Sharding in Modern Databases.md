---
tags: [system-design, hld, scaling, database]
aliases: ["Sharding in Modern Databases", "Database Sharding"]
---

# Sharding in Modern Databases

1) Most modern distributed databases handle sharding automatically.
2) Common NoSQL databases like [Cassandra](https://www.hellointerview.com/learn/system-design/deep-dives/cassandra), [DynamoDB](https://www.hellointerview.com/learn/system-design/deep-dives/dynamodb), and [MongoDB](https://www.mongodb.com/) all let you specify a partition key and handle the rest, but they do not all use the same distribution mechanism:
	- Cassandra uses a partitioner (e.g., Murmur3Partitioner) with virtual nodes, which is a form of [[Consistent Hashing|consistent hashing]] to map partition keys to token ranges on nodes.
	- DynamoDB hashes the partition key to route items to internal partitions and splits/merges partitions as they grow; this is not classic ring-based consistent hashing exposed to users.
	- MongoDB shards data into range-based chunks on the shard key. If you choose a hashed shard key, the ranges are over the hash space. A background balancer automatically splits and migrates chunks to keep shards balanced. It is not classic consistent hashing.

3) They automatically rebalance when you add capacity and route queries to the right shards, but the mechanics differ.
4) SQL databases have also matured and made sharding easier than it once was. [Vitess](https://vitess.io/) and [Citus](https://www.citusdata.com/) are popular open-source sharding layers that sit in front of PostgreSQL or MySQL.
5) They handle query routing, cross-shard operations, and resharding without you having to build it yourself.
6) Cloud providers like AWS Aurora and Google Cloud Spanner offer distributed SQL with built-in sharding.

 > [!tip]
 > In interviews, it's enough to say "We'll use DynamoDB with user_id as the partition key" or "We'll shard using Vitess on user_id and plan for operator-driven online resharding." You don't need to implement sharding internals unless you're specifically asked.

## Related

- [[> Sharding]] — back to the Sharding section MOC
- [[Consistent Hashing]] — the distribution mechanism used by Cassandra and others
- [[How to Shard Your Data]] — the strategies these databases implement under the hood
- [[Sharding in System Design]] — how to reference these databases in an interview
