---
tags: [system-design, hld, database-indexing]
aliases: ["How Indexes Work", "Database Index Internals"]
---

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


# How Database Indexes Work

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

## Related

- [[> Database Indexing]] — back to the section MOC
- [[B-Tree Indexes]] — the default index for most use cases
- [[LSM Trees]] — write-optimized alternative to B-trees
