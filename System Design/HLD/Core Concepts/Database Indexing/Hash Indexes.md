---
tags: [system-design, hld, database-indexing]
aliases: ["Hash Index"]
---

# Hash Indexes

While [[B-Tree Indexes]] dominate the indexing landscape, hash indexes serve a specialized purpose: they excel at exact-match queries. They're simply a persistent hashmap implementation, trading flexibility for super-fast O(1) lookups.

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
2) PostgreSQL supports them but doesn't use them by default because [[B-Tree Indexes|B-trees]] perform nearly as well for exact matches while supporting range queries and sorting.
3) As the PostgreSQL documentation notes, "B-trees can handle equality comparisons almost as efficiently as hash indexes."
4) However, hash indexes do shine in specific scenarios, particularly for in-memory databases.
5) Redis, for example, uses hash tables as its primary data structure for key-value lookups because all data lives in memory.
6) MySQL's MEMORY storage engine defaulted to hash indexes because in-memory exact-match queries were its primary use case. 
7) When working with disk-based storage, [[B-Tree Indexes|B-trees]] are usually the better choice due to their efficient handling of disk I/O patterns.

#### When to Choose Hash Indexes
1) For system design interviews, you might consider hash indexes when:
- You need the absolute fastest possible exact-match lookups
- You'll never need range queries or sorting
- You have plenty of memory (hash indexes tend to be larger than B-trees)

1) But in most cases, [[B-Tree Indexes|B-trees]] will be the better choice. 
2) They're nearly as fast for exact matches and give you the flexibility to handle range queries when you need them.
3) In the words of database expert Bruce Momjian: "Hash indexes solve a problem we rarely have."

> [!warning]
> Hash indexes are rarely used in production systems. They're a bit like that specialized kitchen gadget you buy and then use only once. B-trees are just so versatile that they cover most use cases where you might consider a hash index.

## Related

- [[> Database Indexing]] — back to the section MOC
- [[B-Tree Indexes]] — the versatile default that covers almost every hash index use case
- [[LSM Trees]] — write-optimized alternative for different workloads
