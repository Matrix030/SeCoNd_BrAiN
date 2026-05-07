---
tags: [system-design, hld, scaling]
aliases: ["Hash Ring", "Consistent Hashing Algorithm"]
---

# Consistent Hashing

1) Consistent hashing is a technique that solves the problem of data redistribution when adding or removing a instance in a distributed system.
2) The key insight is to arrange both our data and our databases in a circular space, often called a "hash ring."

Here's how it works:
1. We first create a hash ring with a fixed number of points. To keep it simple, let's say 100.
2. We then place our database nodes on the hash ring. In the case where we have 4 databases, we could put them at points 0, 25, 50, and 75.
3. In order to know which database an event should be stored on, we first hash the event ID like we did before, but instead of using modulo, we just find the hash value on the ring and then move clockwise until we find a database instance.

> [!warning]
> In reality, a hash ring usually has a hash space of 0 to 2^32 - 1 not 0-100, but the concept is the same.

How did this solve our problem? Let's look at what happens when we add or remove a database:

**Adding a Database (Database 5)** Let's say we add a fifth database at position 90 on our ring. Now:

- Only events that hash to positions between 75 and 90 need to move
- These events were previously going to DB1 (at position 0)
- All other events stay exactly where they are!

![[Pasted image 20260505161502.png]]

## Virtual Nodes

1) There is still just one problem.
2) In our example above where we removed database 2, we had to move all events that were stored on database 2 to database 3.
3) Now database 3 has 2x the load of database 1 and database 4.
4) We'd much prefer if we could spread the load more evenly so database 3 wasn't overloaded.

The **solution** is to use what are called **"virtual nodes"**. 
- Instead of putting each database at just one point on the ring, we put it at multiple points by hashing different variations of the database name.
![[Pasted image 20260505161632.png]]

1) For example, instead of just hashing "DB1" to get position 0, we hash "DB1-vn1", "DB1-vn2", "DB1-vn3", etc., which might give us positions 20, 35, 65 and so on. 
2) We do this for each database, which results in the virtual nodes being naturally intermixed around the ring.

Now when Database 2 fails:
- The events that were mapped to "DB2-vn1" will be redistributed to Database 1
- The events that were mapped to "DB2-vn2" will go to Database 3
- The events that were mapped to "DB2-vn3" will go to Database 4
- And so on...

3) This means the load from the failed database gets distributed much more evenly across all remaining databases instead of overwhelming just one neighbor. 
4) The more virtual nodes you use per database, the more evenly distributed the load becomes.
5) Virtual nodes also help when adding a new database. 
6) Without virtual nodes, a new node only takes load from its single clockwise neighbor.
7) With virtual nodes, the new node's virtual positions are spread around the ring, so it absorbs a small chunk of data from multiple existing nodes.
8) This gives you a much more balanced cluster from the start, rather than only relieving one overloaded neighbor.

## Related

- [[> Consistent Hashing]] — back to the section MOC
- [[Simple Modulo Hashing]] — the problem this solves
- [[Addressing Hot Spots]] — handling uneven traffic even after even key distribution
- [[Sharding]] — broader data distribution strategies
