---
tags: [system-design, hld, data-modeling]
aliases: ["Wide-Column Store", "Cassandra", "Column Family"]
---

# Wide-Column Databases

Wide-column databases organize data into column families where rows can have different sets of columns. They're optimized for massive write-heavy workloads and time-series data.

When a user creates a new post, you add a new row keyed by (user_id, timestamp). Rows with the same partition key (user_id) are stored together, making writes fast (just append to the partition) and reads efficient (scan a contiguous range of a user's posts).

![[Pasted image 20260424132020.png]]

**When to consider over SQL:**
- When you have enormous write volumes, time-series data, or analytics workloads where you primarily append data and run aggregations.
- Think telemetry, event logging, or IoT sensor data.

**Data modeling impact:** 
- You'll design around query patterns even more than with SQL, often duplicating data across different column families to support various access patterns.
- Time becomes a first-class citizen in your modeling.

**Example technologies include** [[Cassandra]] and [HBase](https://hbase.apache.org/).

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Database Model Options]] — when to pick wide-column over SQL
- [[Scaling and Sharding]] — wide-column databases are built for horizontal scale
