---
tags: [system-design, hld, database-indexing]
aliases: ["LSM Tree", "Log-Structured Merge Tree", "Log-Structured Merge Trees"]
---

# LSM Trees (Log-Structured Merge Trees)

[[B-Tree Indexes|B-trees]] are great for balanced workloads, but what happens when you need to handle tons of writes?
1) Think about building a system like DataDog that's ingesting millions of metrics per second from thousands of servers.
2) Every CPU reading, memory stat, and error count needs to be stored immediately.
3) With [[B-Tree Indexes|B-trees]], each write means finding the right leaf page, reading it into memory, updating it, and writing it back to disk. 
4) For a few thousand writes per second, this works fine.
5) But when you're processing 100,000 writes per second, those random disk seeks become a bottleneck.
6) It's like trying to update a filing cabinet where you need to find and modify individual folders scattered throughout - eventually, you spend more time searching than actually storing data.
7) This is where LSM trees work really well. But they take a fundamentally different approach to indexing.
8) With [[B-Tree Indexes|B-trees]], each index is its own separate structure that you can create on any column. LSM trees don't work this way.
9) The LSM tree is the storage format for your entire table, sorted by the primary key.
10) Your primary key lookups are extremely fast, but secondary indexes require additional structures - some systems like Cassandra and DynamoDB (via GSIs/LSIs) support them, though with different performance characteristics than primary key access.

Instead of updating data in place like [[B-Tree Indexes|B-trees]], LSM trees use an append-only approach that's built for write-heavy workloads.

#### How LSM Trees Work

1) LSM trees solve the write problem by batching writes in memory and flushing them to disk sequentially.
2) Instead of immediately writing each update to disk like [[B-Tree Indexes|B-trees]] do, LSM trees buffer changes in memory and write them out in large chunks. 
3) This converts many small random writes into fewer large sequential writes, increasing efficiency.
4) Here's what happens when you write to a database that uses LSM trees:
	1. **Memtable (Memory Component)**: New writes go into an in-memory structure called a memtable, typically implemented as a sorted data structure like a red-black tree or skip list. This is extremely fast since it's all in RAM.
	2. **Write-Ahead Log (WAL)**: To ensure durability, every write is also appended to a write-ahead log on disk. This is a sequential append operation, which is much faster than random writes.
	3. **Flush to SSTable**: Once the memtable reaches a certain size (often a few megabytes), it's frozen and flushed to disk as an immutable Sorted String Table (SSTable). This is a single sequential write operation that can write megabytes of data at once.
	4. **Compaction**: Over time, you accumulate many SSTables on disk. A background process called compaction periodically merges these files, removing duplicates and deleted entries. This keeps the number of files manageable and maintains read performance.

![[Pasted image 20260503182426.png]]

5) This makes writes incredibly fast, you're just appending to memory and a log file.
6) Even when flushing to disk, you're writing large sequential chunks rather than seeking to random locations.

#### Negative Impact on Reads

1) As always, this benefits comes at a cost.
2) While LSM trees excel at writes, they make reads more complex.
3) Remember how [[B-Tree Indexes|B-trees]] could find any record with just 2-3 disk reads? With LSM trees, the story is different.
4) When you query for a specific key, the database must check multiple places:
	1. First, the memtable: Is the data in the current in-memory buffer?
	2. Then, immutable memtables: Any memtables waiting to be flushed?
	3. Finally, all SSTables on disk: Starting from the newest (most likely to have recent data) and working backwards

5) This means a single point query might need to check dozens of files in the worst case. It's like searching for a document that could be in your desk drawer, filing cabinet, or any of several archive boxes. And you have to check them all.

Obviously, this would make LSM trees almost unusable for any workflow requiring reasonable read performance. So to mitigate this problem, LSM trees typically employ several optimizations:

1. Bloom Filters:
	- Each SSTable has an associated bloom filter - a probabilistic data structure that can quickly tell you if a key is definitely NOT in that file.
	- This lets you skip most SSTables without reading them. 
	- If the bloom filter says "maybe", you still need to check, but it eliminates the definite misses.
2. Sparse Indexes:
	- Since SSTables are sorted, they maintain sparse indexes that tell you the range of keys in each block.
	- If you're looking for user_id=500 and an SSTable only contains keys 1000-2000, you can skip it entirely.
3. Compaction Strategies:
	- Different compaction strategies optimize for different workloads. 
	- Size-tiered compaction minimizes write amplification but can lead to more files to check.
	- Leveled compaction maintains fewer files but requires more frequent rewrites.

Despite these optimizations, LSM trees fundamentally trade read performance for write performance. This makes them perfect for **write-heavy workloads** like time-series databases, logging systems, and analytics platforms where you're constantly ingesting new data but queries are less frequent or can tolerate slightly higher latency.

The **key insight** for system design is knowing when this trade-off makes sense. If you're building a system that writes far more than it reads - like a metrics collection system, audit log, or IoT data platform - LSM trees are likely the right choice. But for a user-facing application where every page load triggers multiple queries, [[B-Tree Indexes|B-trees]] usually perform better.

#### Real-World Examples

LSM trees power some of the most write-heavy systems on the internet:

1) **Cassandra** handles Netflix's billions of viewing events. When you watch a show, that data gets written to Cassandra's LSM-based storage without slowing down playback.
2) **RocksDB** (built by Facebook) serves as the storage engine for many databases. It handles millions of social interactions per second—likes, posts, messages—all written to LSM trees for fast persistence.
3) **DynamoDB** is generally understood to use an LSM-tree–style storage architecture optimized for high write throughput; its exact internals aren't exposed publicly, and it does not dynamically switch storage engines based on access patterns.

## Related

- [[> Database Indexing]] — back to the section MOC
- [[B-Tree Indexes]] — the read-optimized alternative; default for balanced workloads
- [[Hash Indexes]] — another specialized index type
