---
tags: [system-design, hld, database-indexing]
aliases: ["B-Tree Index", "B-Trees", "B+ Tree"]
---

# B-Tree Indexes

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
7) Its storage internals aren't publicly documented in detail, but it's widely understood to use an [[LSM Trees|LSM-style storage architecture]] rather than a B-tree for its underlying engine.
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

## Related

- [[> Database Indexing]] — back to the section MOC
- [[LSM Trees]] — write-optimized alternative for heavy write workloads
- [[Hash Indexes]] — O(1) exact-match lookups; rarely beats B-trees in practice
- [[Composite Indexes]] — using B-trees across multiple columns for real query patterns
