---
tags: [system-design, hld, data-modeling]
aliases: ["Key-Value Store", "Redis", "KV Store"]
---

# Key-Value Stores

- Key-value stores provide simple lookups where you fetch values by exact key match. 
- They're extremely fast but offer limited query capabilities beyond that basic operation.

**When to consider over SQL:** 
- For caching, session storage, feature flags, or any scenario where you only need to look up data by a single identifier.
- They're also good for high-write scenarios where you need maximum performance and don't need complex queries.

> [!tip]
> "Over SQL" is misleading here. In practice, you'll often use both together. SQL as your source of truth with a key-value cache (like Redis) in front for hot data. This gives you fast access without sacrificing durability or complex queries.

**Data modeling impact:** 
- Your schema becomes very flat. 
- You'll denormalize heavily and duplicate data across multiple keys to support different access patterns, since you can't join or query across relationships. 
- This is great for reads but terrible for consistency when data changes.

![[Pasted image 20260424131928.png]]

**Example technologies include** Redis, DynamoDB, Memcached.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Database Model Options]] — when to pick key-value over SQL
- [[Relational Databases]] — the SQL source of truth alongside KV caches
- [[Normalization vs Denormalization]] — key-value stores require heavy denormalization
