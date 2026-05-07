---
tags: [system-design, hld, scaling]
aliases: ["Hot Spots"]
---

# Addressing Hot Spots

1) Even with virtual nodes distributing data evenly, hot spots can still occur. 
2) A hot spot is when one node gets a disproportionate amount of traffic because certain keys are far more popular than others.
3) Think of a ticketing system where a Taylor Swift concert generates 100x the reads of any other event.

4) Consistent hashing doesn't solve this on its own since it distributes keys evenly, not traffic. Here are a few strategies that real systems use:
	
	- Read replicas: Replicate popular keys across multiple nodes and load-balance reads among them. This is the most common approach.
	- Key-space salting: Append a random suffix to hot keys (e.g., taylor-swift-{0..9}) so they hash to different nodes. Reads then scatter across those nodes and get aggregated.
	- Adaptive rebalancing: Monitor traffic in real-time and move specific key ranges off overloaded nodes. This is operationally complex but some systems (like DynamoDB) do it automatically.

The key distinction to make is: virtual nodes prevent structural imbalance (uneven key distribution), while replication and key salting prevent workload imbalance (uneven traffic).

## Related

- [[> Consistent Hashing]] — back to the section MOC
- [[The Hash Ring]] — how consistent hashing distributes keys
- [[> Caching]] — read replicas as a caching strategy
