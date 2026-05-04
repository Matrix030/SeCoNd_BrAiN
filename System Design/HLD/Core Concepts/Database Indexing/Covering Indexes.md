---
tags: [system-design, hld, database-indexing, database-optimization]
aliases: ["Covering Index"]
---

# Covering Indexes

1) A covering index is one that includes all the columns needed by your query - not just the columns you're filtering or sorting on.
2) Think about showing a social media feed with post timestamps and like counts.
3) With a regular index on (user_id, created_at), the database first finds matching posts in the index, then has to fetch each post's full data page just to get the like count. 
4) That's a lot of extra disk reads just to display a number.

By including the likes column directly in our index, we can skip those expensive page lookups entirely. The database can return everything we need straight from the index itself:

```
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    user_id INT,
    title TEXT,
    content TEXT,
    likes INT,
    created_at TIMESTAMP
);

-- Regular index
CREATE INDEX idx_user_time ON posts(user_id, created_at);

-- Covering index includes likes column
CREATE INDEX idx_user_time_likes ON posts(user_id, created_at) INCLUDE (likes);
```


> [!info]
> I'm using SQL as the examples given it's the most ubiquitous language for database interactions. But the same principles apply to other database systems and even NoSQL solutions.

5) With the covering index, PostgreSQL can return results purely from the index data - no need to look up each post in the main table. 
6) This is especially powerful for queries that only need a small subset of columns from large tables.
7) The trade-off is, of course, size - covering indexes are larger because they store extra columns. 
8) But for frequently-run queries that only need a few columns, the performance boost from avoiding table lookups often justifies the storage cost. 
9) This is particularly true in social feeds, leaderboards, and other read-heavy features where query speed is critical.

> [!warning]
> The reality in 2026 is that covering indexes are more of a niche optimization than a go-to solution. Modern database query optimizers have become quite smart at executing queries efficiently with regular indexes. While covering indexes can provide significant performance gains in specific scenarios - like read-heavy tables with limited columns - they come with real costs in terms of maintenance overhead and storage space.
> 
> In an interview, you may be wise to focus on simpler indexing strategies and, if reaching for covering indexes, be sure to make sure you have a good reason for why it's necessary.
> 
> If you're not sure if they make sense in a given scenario, it's often better to err on the side of simplicity.

## Related

- [[> Database Indexing]] — back to the section MOC
- [[Composite Indexes]] — the foundation; covering indexes extend composite indexes further
- [[B-Tree Indexes]] — the underlying structure both composite and covering indexes use
