---
tags: [system-design, hld, database-indexing]
aliases: ["Database Indexing Summary"]
---

# Wrapping Up

![[Pasted image 20260504151026.png]]

1) Indexes matter. Not just in interviews, but in production systems.
2) Knowing how to use them effectively is a key skill for any developer and is knowledge that is regularly tested in interviews.
3) Most important is knowing when you need an index, and on what columns.
4) This should be a natural instinct when you're designing a new schema.
5) Consider the query patterns you're likely to run, and whether you'll be filtering or sorting on certain columns.
6) From here, expect that you may be asked what type of index you would use for a given scenario. When in doubt, go with [[B-Tree Indexes|B-trees]].
7) They're the swiss army knife of indexes, handling both exact matches and range queries efficiently, and they're what most databases use by default for good reason.

The two main exceptions are when you're dealing with spatial data, or full-text search.

If you're dealing with latitude and longitude, and need to efficiently search for nearby points, you'll want to opt for a [[Geospatial Indexes|geospatial index]]. If you only want to know one option, learn [[Geospatial Indexes#Geohash|geohashing]]. Better still if you can explain how it works and weigh the tradeoffs between it and tree-based approaches.

Lastly, when it comes to full-text search, you'll need an [[Inverted Indexes|inverted index]] to search large amounts of text efficiently. While it's unlikely you'll get deeply probed about how they work, you should have a basic understanding of the reverse mapping from keywords to documents.

With these tools in your toolbelt, you'll be well prepared for the overwhelming majority of indexing questions that may come your way.

## Related

- [[> Database Indexing]] — back to the section MOC
- [[B-Tree Indexes]] — the default index for most scenarios
- [[Geospatial Indexes]] — for proximity and location queries
- [[Inverted Indexes]] — for full-text search
