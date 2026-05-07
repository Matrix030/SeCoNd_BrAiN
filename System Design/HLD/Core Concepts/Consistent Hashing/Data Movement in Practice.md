---
tags: [system-design, hld, scaling, fault-tolerance]
aliases: ["Data Movement"]
---

# Data Movement in Practice

> [!info]
> Consistent hashing tells you where data should live, but it doesn't magically teleport terabytes of data when a node goes down. In practice, most distributed databases use replication alongside consistent hashing to handle failures without moving data at all.
> 
> For example, DynamoDB replicates each partition across three availability zones. When a primary node fails, a replica is promoted via a consensus algorithm like Raft, and no data needs to move. Cassandra works similarly, replicating data to N consecutive nodes on the ring so reads can be served from surviving replicas.
> 
> Data movement really only happens during planned membership changes like adding capacity or permanently replacing a node to restore the replication factor. Even then, consistent hashing ensures only a bounded fraction of keys need to be re-replicated, not the entire dataset.

## Related

- [[> Consistent Hashing]] — back to the section MOC
- [[The Hash Ring]] — how the ring determines where data lives
- [[Addressing Hot Spots]] — related node-level challenges
