---
tags: [system-design, hld, scaling, database]
aliases: ["Partitioning", "Horizontal Partitioning", "Vertical Partitioning"]
---

# First, what is Partitioning?

1) Partitioning means splitting a large table into smaller pieces inside a single database instance. 
2) It does not add more machines. 
3) Instead it organizes data so the database can work more efficiently.
	- Consider an orders table with 500 million rows and 2 TB of data.
	- A query for last month's orders has to scan the entire table. 
	- Indexes become huge and slow to maintain while routine operations like vacuuming, analyzing, or rebuilding indexes can lock the whole table and impact performance.

4) Partitioning solves this problem by breaking that large table into smaller partitions.
5) The data does not move off the machine.
6) It is simply divided into logical pieces the database can manage separately.
7) Now a query for last month's orders only scans the relevant partition instead of the full table.

There are two common types of partitioning:

- **Horizontal partitioning**: Split rows across partitions. For example, one partition per year of orders. Same columns, fewer rows per partition.

- **Vertical partitioning**: Split columns across partitions. For example, keep frequently accessed columns in one partition and large or rarely used columns in another. Same rows, fewer columns per partition.

## Related

- [[> Sharding]] — back to the Sharding section MOC
- [[What is Sharding]] — sharding is horizontal partitioning across multiple machines
- [[> Database Indexing]] — how indexes relate to partitioned tables
