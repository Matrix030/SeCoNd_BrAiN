1) Database performance can make or break modern applications. 
2) Think about what it takes to search for a user's profile by email in a table with millions of records.
3) Without any optimizations, the database would have to check each row sequentially, scanning through every single record until it finds a match. 
4) For a table with millions of rows, this becomes painfully slow - like searching through every book in a library one by one to find a specific novel.
5) By maintaining separate data structures optimized for searching, indexes allow databases to quickly locate the exact records we need without examining every row.
6) From finding products in an e-commerce catalog to loading user profiles in a social network, indexes are what make fast lookups possible.
7) Knowing when to add an index, to what columns, and what type of index is a critical part of system design.
8) Choosing the right indexes is often a key focus.
9) For staff-level engineers, mastery of different index types and their trade-offs is essential.
First, let's understand exactly how databases store and use indexes to make our queries efficient.

> [!info]
> Indexing and data organization tends to be a stronger focus in infrastructure style interviews. For full-stack and product-focused roles, you'll likely only need a basic understanding of when and why to use indexes. The depth we cover here goes beyond what's typically asked in full-stack interviews, but understanding the fundamentals will help you make better decisions when designing and optimizing your applications.


## How Database Indexes Work

1) When we store data in a database, it's ultimately written to disk as a collection of files.
2) The main table data is typically stored as a heap file - essentially a collection of rows in no particular order.
3) Think of this like a notebook where you write entries as they come, one after another.

### Physical Storage and Access Patterns

> [!warning]
> Unless interviewing for a database internals role, the details here are not going to be asked in an interview. That said, they are an important foundation to understand the problem of why we need indexes.

1) Modern databases face an interesting challenge: they need to store and quickly access vast amounts of data.
2) While the data lives on disk (typically SSDs nowadays), we can only process it when it's in memory.
3) This means every query requires loading data from disk into RAM.
4) When querying without an index, we need to scan through every page of data one by one, loading each into memory to check if it contains what we're looking for.
5) With millions of pages, this means millions of (relatively) slow disk reads just to find a single record. It's like having to flip through every page of a massive book to find one specific word.

> [!info]
> Modern databases have optimizations like prefetching and caching to make random access faster, but the point here still stands. It's too slow to scan through every page of data sequentially.

6) But with indexes, we transform our access patterns.
7) Instead of reading through every page of data sequentially, indexes provide a structured path to follow directly to the data we need.
8) They help us minimize the number of pages we need to read from storage by telling us exactly which pages contain our target data.
9) It's the difference between checking every page in a book versus using the table of contents to jump straight to the relevant pages.

> [!info]
> While SSDs are the norm today, it's important to note that random access is still significantly slower than sequential access, even on SSDs. This is a common misconception - while the performance gap is smaller than with HDDs, it's still very real. And for systems still using HDDs, especially for large datasets, this performance difference becomes even more pronounced, making proper indexing absolutely critical.

### Cost
1) Indexes aren't free:
	- they come with their own set of trade-offs. Every index we create requires additional disk space, sometimes nearly as much as the original data.
	- Write performance takes a hit too. When we insert a new row or update an existing one, the database must update not just the main table, but every index on it. With multiple indexes, a single write operation can trigger several disk writes.
	- The classic case is a table with frequent writes but infrequent reads.
	- Think of a logging table where we're constantly inserting new records but rarely querying old ones.
	- Here, the overhead of maintaining indexes might not justify their benefit.
	- Similarly, for small tables with just a few hundred rows, the cost of maintaining an index and traversing its structure might exceed the cost of a simple sequential scan.

> [!tip]
> In reality, the impact of indexes on memory is often overblown. Modern databases have smart buffer pool management that reduces the performance hit of having multiple indexes. However, it's still a good idea to closely monitor index usage and avoid creating unnecessary indexes that don't provide significant benefits.

## Types of indexes

1) There are lots of indexes, many of which fall into the tail and are rarely used but for specialized use cases.
2) Rather than enumerating every type of index you may see in the wild, we're going to focus in on the most common ones that show up in system design interviews.
### B-Tree Indexes

1) B-tree indexes are the most common type of database index, providing an efficient way to organize data for fast searches and updates.
2) They achieve this by maintaining a balanced tree structure that minimizes the number of disk reads needed to find any piece of data.

#### The Structure of B-trees

1) A B-tree is a self-balancing tree that maintains sorted data and allows for efficient insertions, deletions, and searches.
2) Unlike binary trees where each node has at most two children, B-tree nodes can have multiple children - typically hundreds in practice. 
3) Each node contains an ordered array of keys and pointers, structured to minimize disk reads.

![[Pasted image 20260503173721.png]]

4) Every node in a B-tree follows strict rules:
- All leaf nodes must be at the same depth
- Each node can contain between m/2 and m keys (where m is the order of the tree)
- A node with k keys must have exactly k+1 children
- Keys within a node are kept in sorted order

4) This structure is particularly clever because it maps perfectly to how databases store data on disk.
5) Each node is sized to fit in a single disk page (typically 8KB), maximizing our I/O efficiency.
6) When PostgreSQL needs to find a record with id=350, it might only need to read 2-3 pages from disk: the root node, maybe an internal node, and finally a leaf node.

#### Real-World Examples

1) B-trees are everywhere in modern databases.
2) PostgreSQL uses them for almost everything - primary keys, unique constraints, and most regular indexes are all B-trees.
3) When you create a table like this in PostgreSQL:

```
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE
);
```

4) PostgreSQL automatically creates two B-tree indexes: one for the primary key and one for the unique email constraint.
5) These B-trees maintain sorted order, which is crucial for both uniqueness checks and range queries.
6) DynamoDB organizes items within a partition in sort-key order, enabling efficient range queries within that partition.
7) Its storage internals aren’t publicly documented in detail, but it’s widely understood to use an LSM-style storage architecture rather than a B-tree for its underlying engine.
8) Even MongoDB, with its document model, uses B-trees (specifically B+ trees, a variant where all data is stored in leaf nodes) for its indexes.
9) When you create an index in MongoDB like this:

```
db.users.createIndex({ "email": 1 });
```

You're creating a B-tree that maps email values to document locations.

#### Why B-trees are the default choice

- B-trees have become the default choice for most database indexes because they excel at everything databases need:
	1. They maintain sorted order, making range queries and ORDER BY operations efficient
	2. They're self-balancing, ensuring predictable performance even as data grows
	3. They minimize disk I/O by matching their structure to how databases store data
	4. They handle both equality searches (email = 'x') and range searches (age > 25) equally well
	5. They remain balanced even with random inserts and deletes, avoiding the performance cliffs you might see with simpler tree structures

- If you find yourself in an interview and you need to decide which index to use, B-trees are a safe bet.

### LSM Trees (Log-Structured Merge Trees)

B-trees are great for balanced workloads, but what happens when you need to handle tons of writes?
1) Think about building a system like DataDog that's ingesting millions of metrics per second from thousands of servers.
2) Every CPU reading, memory stat, and error count needs to be stored immediately.
3) With B-trees, each write means finding the right leaf page, reading it into memory, updating it, and writing it back to disk. 
4) For a few thousand writes per second, this works fine.
5) But when you're processing 100,000 writes per second, those random disk seeks become a bottleneck.
6) It's like trying to update a filing cabinet where you need to find and modify individual folders scattered throughout - eventually, you spend more time searching than actually storing data.
7) This is where LSM trees work really well. But they take a fundamentally different approach to indexing.
8) With B-trees, each index is its own separate structure that you can create on any column. LSM trees don't work this way.
9) The LSM tree is the storage format for your entire table, sorted by the primary key.
10) Your primary key lookups are extremely fast, but secondary indexes require additional structures - some systems like Cassandra and DynamoDB (via GSIs/LSIs) support them, though with different performance characteristics than primary key access.

Instead of updating data in place like B-trees, LSM trees use an append-only approach that's built for write-heavy workloads.

#### How LSM Trees Work

1) LSM trees solve the write problem by batching writes in memory and flushing them to disk sequentially.
2) Instead of immediately writing each update to disk like B-trees do, LSM trees buffer changes in memory and write them out in large chunks. 
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
3) Remember how B-trees could find any record with just 2-3 disk reads? With LSM trees, the story is different.
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

The **key insight** for system design is knowing when this trade-off makes sense. If you're building a system that writes far more than it reads - like a metrics collection system, audit log, or IoT data platform - LSM trees are likely the right choice. But for a user-facing application where every page load triggers multiple queries, B-trees usually perform better.

#### Real-World Examples

LSM trees power some of the most write-heavy systems on the internet:

1) **Cassandra** handles Netflix's billions of viewing events. When you watch a show, that data gets written to Cassandra's LSM-based storage without slowing down playback.
2) **RocksDB** (built by Facebook) serves as the storage engine for many databases. It handles millions of social interactions per second—likes, posts, messages—all written to LSM trees for fast persistence.
3) **DynamoDB** is generally understood to use an LSM-tree–style storage architecture optimized for high write throughput; its exact internals aren’t exposed publicly, and it does not dynamically switch storage engines based on access patterns.

### Hash Indexes

While B-trees dominate the indexing landscape, hash indexes serve a specialized purpose: they excel at exact-match queries. They're simply a persistent hashmap implementation, trading flexibility for super-fast O(1) lookups.

#### How Hash Indexes Work

1) At their core, hash indexes are just a hashmap that maps indexed values to row locations.
2) The database maintains an array of buckets, where each bucket can store multiple key-location pairs.
3) When indexing a value, the database hashes it to determine which bucket should store the pointer to the row data.
![[Pasted image 20260503183427.png]]

For example, with a hash index on email:

```
buckets[hash("alice@example.com")] -> [ptr to page 1]
buckets[hash("bob@example.com")]   -> [ptr to page 2]
```

4) Hash collisions are handled by allowing multiple entries per bucket, many systems use chaining with overflow storage when a bucket fills.
5) For example, PostgreSQL hash indexes use buckets that can link to overflow pages (chaining). With a good hash function and load factor, average lookups remain O(1).
6) This structure makes hash indexes incredibly fast for exact-match queries - just compute the hash, go to the bucket, and follow the pointer.
7) However, this same structure makes them useless for range queries or sorting since similar values are deliberately scattered across different buckets.

#### Real-World Usage

1) Despite their speed for exact matches, hash indexes are relatively rare in practice.
2) PostgreSQL supports them but doesn't use them by default because B-trees perform nearly as well for exact matches while supporting range queries and sorting.
3) As the PostgreSQL documentation notes, "B-trees can handle equality comparisons almost as efficiently as hash indexes."
4) However, hash indexes do shine in specific scenarios, particularly for in-memory databases.
5) Redis, for example, uses hash tables as its primary data structure for key-value lookups because all data lives in memory.
6) MySQL's MEMORY storage engine defaulted to hash indexes because in-memory exact-match queries were its primary use case. 
7) When working with disk-based storage, B-trees are usually the better choice due to their efficient handling of disk I/O patterns.
#### When to Choose Hash Indexes
1) For system design interviews, you might consider hash indexes when:
- You need the absolute fastest possible exact-match lookups
- You'll never need range queries or sorting
- You have plenty of memory (hash indexes tend to be larger than B-trees)

1) But in most cases, B-trees will be the better choice. 
2) They're nearly as fast for exact matches and give you the flexibility to handle range queries when you need them.
3) In the words of database expert Bruce Momjian: "Hash indexes solve a problem we rarely have."

> [!warning]
> Hash indexes are rarely used in production systems. They're a bit like that specialized kitchen gadget you buy and then use only once. B-trees are just so versatile that they cover most use cases where you might consider a hash index.

### Geospatial Indexes

Here's an interesting quirk of system design interviews:
- while geospatial indexes are fairly specialized in practice - you might never touch them unless you're working with location data - they've become a common focus in interviews.
- Why? The rise of location-based services like Uber, Yelp, and Find My Friends has made proximity search a favorite interview topic.

#### The Challenge with Location Data

1) Say we're building a restaurant discovery app like Yelp.
2) We have millions of restaurants in our database, each with a latitude and longitude. 
3) A user opens the app and wants to find "restaurants within 5 miles of me." Seems simple enough, right?
4) The naive approach would be to use standard B-tree indexes on latitude and longitude:

```
CREATE TABLE restaurants (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    latitude DECIMAL(10, 8),
    longitude DECIMAL(11, 8)
);

CREATE INDEX idx_lat ON restaurants(latitude);
CREATE INDEX idx_lng ON restaurants(longitude);
```

5) But this falls apart quickly when we try to execute a proximity search.
6) Think about how a B-tree index on latitude and longitude actually works. 
7) We're essentially trying to solve a 2D spatial problem (finding points within a circle) using two separate 1D indexes.
8) When we query "restaurants within 5 miles," we'll inevitably hit one of two performance problems:
	- If we use the latitude index first, we'll find all restaurants in the right latitude range - but that's a long strip spanning the entire globe at that latitude!
	- Then for each of those restaurants, we need to check if they're also in the right longitude range.
	- Our index on longitude isn't helping because we're not doing a range scan - we're doing point lookups for each restaurant we found in the latitude range.
	- If we try to be clever and use both indexes together (via an index intersection), the database still has to merge two large sets of results - all restaurants in the right latitude range and all restaurants in the right longitude range. 
	- This creates a rectangular search area much larger than our actual circular search radius, and we still need to filter out results that are too far away.

![[Pasted image 20260503190442.png]]

9) This is why we need indexes that understand 2D spatial relationships. Rather than treating latitude and longitude as independent dimensions, spatial indexes let us organize points based on their actual proximity in 2D space.

#### Core Approaches

1) There are three main approaches you'll encounter in interviews: geohashes, quadtrees, and R-trees.
2) Each has its own strengths and trade-offs, but all solve our fundamental problem: they preserve spatial relationships in our index structure.
3) While this seems like a lot of specialized knowledge, mainly want to see that you understand the basic problem (why regular indexes fall short) and can reason about at least one solution.
4) You don't need deep expertise in all three approaches unless you're interviewing for a role that specifically works with location data.

#### Geohash

We'll start with geohash because it's the simplest spatial index to understand and the core idea is beautifully simple: convert a 2D location into a 1D string in a way that preserves proximity.

![[Pasted image 20260503191403.png]]

1) Imagine dividing the world into four squares and labeling them 0-3.
2) Then divide each of those squares into four smaller squares, and so on. 
3) Each division adds more precision to our location description. 
4) A geohash is essentially this process, but using a base32 encoding that creates strings like "dr5ru" for locations. 
5) The longer the string, the more precise the location:
- "9q8y" might represent all of San Francisco
- "9q8yy" narrows it down to the Mission District
- "9q8yyk" might pinpoint a specific city block

6) What makes geohash clever is that locations that are close to each other usually share similar prefix strings.
7) Two restaurants on the same block might have geohashes that start with "9q8yyk", while a restaurant in a different neighborhood might start with "9q8yym".
8) And here's the real elegance: once we've converted our 2D locations into these ordered strings, we can use a regular B-tree index to handle our spatial queries. 
9) Remember how B-trees excel at prefix matching and range queries? That's exactly what we need for proximity searches.

When we index the geohash strings with a B-tree:

```
CREATE INDEX idx_geohash ON restaurants(geohash);
```

10) Finding nearby locations becomes a matter of searching strings with matching prefixes. 
11) If we're looking for restaurants near geohash "9q8yyk", we can do a range scan in our B-tree for entries starting with that prefix. 
12) For radius queries, we also need to include adjacent geohash cells since our search area might span cell boundaries - but this is still just a handful of prefix range scans.
13) This lets us leverage all the optimizations that databases already have for B-trees - no special spatial data structure needed.

This is why Redis's geospatial commands use this approach internally. When you run:

```
GEOADD restaurants -122.4194 37.7749 "Restaurant A"
GEOSEARCH restaurants FROMLONLAT -122.4194 37.7749 BYRADIUS 5 mi
```

> [!info]
> GEORADIUS is deprecated in favor of GEOSEARCH. Redis uses geohash under the hood to efficiently find nearby points.

1) The main limitation of geohash is that locations near each other in reality might not share similar prefixes if they happen to fall on different sides of a major grid division - like two restaurants on opposite sides of a street that marks a geohash boundary. 
2) But for most applications, this edge case isn't significant enough to matter.
3) This elegance - turning a complex 2D problem into simple string prefix matching that can leverage existing B-tree implementations - is why geohash is such a popular choice. 
4) It's easy to understand, implement, and use with existing database systems that already know how to handle strings efficiently.

#### Quadtree

While less common in production databases today, quadtrees represent a fundamental tree-based approach to indexing 2D space that has shaped how we think about spatial indexing. Unlike geohash which transforms coordinates into strings, quadtrees directly partition space by recursively subdividing regions into four quadrants.

Start with one square covering your entire area. When a square contains more than some threshold of points (typically 4-8), split it into four equal quadrants. Continue this recursive subdivision until reaching a maximum depth or achieving the desired point density per node. This spatial partitioning maps to a tree structure:

![[Pasted image 20260503200201.png]]









