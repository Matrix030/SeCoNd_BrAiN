---
tags: [system-design, hld, database-indexing]
aliases: ["Inverted Index", "Full-Text Search Index"]
---

# Inverted Indexes

- While [[B-Tree Indexes]] excel at finding exact matches and ranges, they fall short when we need to search through text content. Consider what happens when you run a query like:

```
SELECT * FROM posts WHERE content LIKE '%database%';
```

- Here, we're looking for posts that contain the word "database" anywhere in their content - not just posts that start or end with it. Even with a [[B-Tree Indexes|B-tree index]] on the content column, the database can't use the index at all. 
- Why? [[B-Tree Indexes|B-tree indexes]] can only help with prefix matches (like 'database%') or suffix matches (if you index the reversed content).
- When the pattern could match anywhere within the text, the database has no choice but to check every character of every post, reading entire text fields into memory to look for matches.

1) This full pattern matching gets exponentially slower as your text content grows. 
2) Each additional character in your posts means more data to scan, more memory to use, and more CPU cycles spent checking patterns.
3) It's like trying to find every mention of a word in a library by reading every book, page by page.
4) An inverted index solves this by flipping the relationship between documents and their content.
5) Instead of storing documents with their words, it stores words with their documents.
6) Think of it like the index at the back of a textbook - rather than reading every page to find mentions of "ACID properties", you can look up "ACID" and find every page that discusses it.

Here's how it works. Consider a simple blogging platform with these posts:

```
doc1: "B-trees are fast and reliable"
doc2: "Hash tables are fast but limited"
doc3: "B-trees handle range queries well"
```

The inverted index creates a mapping:

```
b-trees  -> [doc1, doc3]
fast     -> [doc1, doc2]
reliable -> [doc1]
hash     -> [doc2]
tables   -> [doc2]
limited  -> [doc2]
handle   -> [doc3]
range    -> [doc3]
queries  -> [doc3]
```

7) While this basic mapping shows the core concept, production inverted indexes are much more sophisticated. When systems like Elasticsearch index text, they first run it through an analysis pipeline that processes and enriches the content. This means:
8. Breaking text into tokens (words or subwords)
9. Converting to lowercase
10. Removing common "stop words" like "the" or "and"
11. Often applying stemming (reducing words to their root form)
12. So when a user searches for "Databases", the system can match documents containing "database", "DATABASE", or even "database's". This is why full-text search feels so natural compared to rigid pattern matching.

Modern search systems like Elasticsearch and Lucene build additional features on top of this foundation:

- Term frequency analysis (how often words appear)
- Relevance scoring (which documents best match the query)
- Fuzzy matching (finding close matches like "databas")
- Phrase queries (matching exact sequences of words)

In practice, you'll see inverted indexes whenever advanced text search is needed. When developers search GitHub repositories, when users search Slack message history, or when you search through documentation - they're all using inverted indexes under the hood.

There are still trade-offs, of course.Inverted indexes require substantial storage overhead and careful updating. When a document changes, you need to update entries for every term it contains. But for making text truly searchable, these are trade-offs we're willing to make.

You can learn more about how inverted indexes work in our [Elasticsearch Deep Dive](https://www.hellointerview.com/learn/system-design/deep-dives/elasticsearch).

## Related

- [[> Database Indexing]] — back to the section MOC
- [[B-Tree Indexes]] — the general-purpose index that falls short for full-text search
- [[Geospatial Indexes]] — another specialized index for a different problem domain
