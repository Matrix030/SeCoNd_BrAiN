---
tags: [system-design, hld, scaling, numbers-to-know]
aliases: ["Numbers Every Engineer Should Know", "Hardware Numbers 2026"]
---

# Numbers To Know

## Modern Hardware Limits

**Compute / Memory**
- AWS [M6i.32xlarge](https://aws.amazon.com/ec2/instance-types/m6i/): 512 GiB RAM, 128 vCPUs (general workloads)
- AWS [X1e.32xlarge](https://aws.amazon.com/ec2/instance-types/x1e/): 4 TB RAM (memory-optimized)
- AWS [U-24tb1.metal](https://aws.amazon.com/blogs/aws/ec2-high-memory-update-new-18-tb-and-24-tb-instances/): 24 TB RAM

**Storage**
- AWS [i3en.24xlarge](https://aws.amazon.com/ec2/instance-types/i3en/): 60 TB local SSD
- AWS [D3en.12xlarge](https://aws.amazon.com/ec2/instance-types/d3/): 336 TB HDD
- [S3](https://aws.amazon.com/s3/): effectively unlimited, petabyte-scale standard

**Network**
- 25 Gbps standard within datacenter
- 50–100 Gbps on high-performance instances
- Cross-AZ bandwidth limited only by instance NIC

**Latency**
- Sub-1ms within an AZ
- 1–2ms across AZs in the same region
- 50–150ms cross-region

## [[Caching]]

- Memory: up to 1 TB on memory-optimized instances
- Read latency: < 1ms within the same region
- Write latency: < 1ms same-AZ, 1–2ms cross-AZ
- Throughput: 100k–200k+ ops/sec per instance (ElastiCache Redis on Graviton)

**When to consider scaling**
- Dataset size approaching 1 TB
- Sustained throughput of 100k+ ops/sec
- Read latency requirements below 0.5ms

## Databases

- Storage: up to 64 TiB per instance for most engines; Aurora up to 256 TiB
- Read latency: 1–5ms cached, 5–30ms disk
- Write latency: 5–15ms commit
- Read throughput: up to 50k TPS single-node (Aurora/RDS)
- Write throughput: 10–20k TPS single-node (Aurora/RDS)
- Connections: 5–20k concurrent

**When to consider [[Sharding|sharding]]**
- Dataset approaching or exceeding 50 TiB
- Write throughput consistently > 10k TPS
- Read latency requirements below 5ms uncached
- Cross-region replication / geographic distribution
- Backup windows stretching into hours

> [!info]
> A "single instance" doesn't mean a single point of failure. In practice, you'd still run a primary with read replicas for availability (e.g. Aurora's multi-AZ failover). The point here is that you often don't need to _shard_ the data — replication for HA is a separate concern from horizontal partitioning for scale.

## Application Servers

- Connections: 100k+ concurrent per instance
- CPU: 8–64 cores
- Memory: 64–512 GB standard, up to 2 TB on high-memory instances
- Network: 25 Gbps standard, 50–100 Gbps on high-performance
- Startup time: 30–60s for containerized apps

**When to consider horizontal scaling**
- CPU consistently > 70–80%
- Response latency exceeding SLA
- Memory > 70–80%
- Network bandwidth approaching instance limit

## Message Queues

- Throughput: up to 1M messages/sec per broker
- Latency: 1–5ms end-to-end within a region
- Message size: 1 KB–10 MB efficiently handled
- Storage: up to 50 TB per broker
- Retention: weeks to months, depending on disk

**When to consider scaling**
- Throughput nearing 800k msgs/sec per broker
- Partition count approaching 200k per cluster
- Consistently growing consumer lag
- Cross-region replication required

Reference: Kafka [benchmarked at 2M writes/sec on three cheap machines](https://engineering.linkedin.com/kafka/benchmarking-apache-kafka-2-million-writes-second-three-cheap-machines).

## Cheat Sheet

|Component|Key Metrics|Scale Triggers|
|---|---|---|
|**Caching**|- ~1ms latency  <br>- 100k+ ops/sec  <br>- Memory-bound (up to 1 TB)|- Hit rate < 80%  <br>- Latency > 1ms  <br>- Memory > 80%  <br>- Cache churn/thrashing|
|**Databases**|- Up to 50k TPS  <br>- Sub-5ms read latency (cached)  <br>- 64 TiB+ storage|- Write throughput > 10k TPS  <br>- Read latency > 5ms uncached  <br>- Geographic distribution needs|
|**App Servers**|- 100k+ concurrent connections  <br>- 8–64 cores @ 2–4 GHz  <br>- 64–512 GB RAM standard, up to 2 TB|- CPU > 70%  <br>- Response latency > SLA  <br>- Connections near 100k/instance  <br>- Memory > 80%|
|**Message Queues**|- Up to 1M msgs/sec per broker  <br>- Sub-5ms end-to-end latency  <br>- Up to 50 TB storage|- Throughput near 800k msgs/sec  <br>- Partition count ~200k per cluster  <br>- Growing consumer lag|

## Common Mistakes

### Premature [[Sharding|sharding]]

- Yelp example: 10M businesses × 1KB = 10 GB. With reviews, ~100 GB. No sharding needed.
- LeetCode leaderboard: 100k competitions × 100k users × (36B ID + 4B float) = 400 GB. Fits on a single large cache.

> [!warning]
> Slow down, do the math, and make sure sharding is actually needed before explaining how you'd do it.

### Overestimating SSD latency

- Indexed row lookups on SSD: sub-millisecond to a few milliseconds.
- No need to add a caching layer for simple indexed lookups.

> [!warning]
> Only true for simple indexed row lookups. Still cache expensive queries.

### Over-engineering high write throughput

- A well-tuned Postgres instance handles 20k+ simple writes/sec.
- 5k WPS does not justify a message queue.
- Real write limits come from: complex multi-table transactions, write amplification from excessive indexes, expensive cascading updates, heavy concurrent reads.
- Reach for a message queue when: guaranteed delivery on downstream failure, event sourcing, write spikes > 20k WPS, decoupling producers from consumers.
- Simpler optimizations first: batch writes, schema/index tuning, connection pooling, async commits.

## Costs

- Interviews rarely focus on exact dollar costs — order-of-magnitude is enough.
- Don't memorize AWS pricing tables.
- Still flag obvious waste: 100 machines when 1 will do, in-memory caches for non-latency-critical data.

## Key Takeaways

- Single databases can handle terabytes of data.
- Caches can hold entire datasets in memory.
- Message queues are fast enough for synchronous flows (no backlog).
- Application servers have enough memory for significant local optimization.

## Related

- [[> Core Concepts]] — back to the section index
- [[Sharding]] — when scale actually demands horizontal partitioning
- [[Caching]] — strategies for leveraging in-memory speed
- [[CAP Theorem]] — the tradeoffs that shape distributed design
- [[Database Indexing]] — why indexed lookups are sub-millisecond fast
