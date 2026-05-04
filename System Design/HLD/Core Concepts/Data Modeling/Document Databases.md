---
tags: [system-design, hld, data-modeling]
aliases: ["Document Store", "MongoDB"]
---

# Document Databases

1) Document databases store data as JSON-like documents with flexible schemas, making them good for rapidly evolving applications where you don't know all your data fields upfront. 
2) Your data modeling becomes more about nesting and embedding related information within documents rather than normalizing across tables.
3) Notice how the same data from our SQL example gets restructured. 
4) Instead of separate tables, we embed posts directly within each user document. 
5) This eliminates joins but means updating a post requires finding and modifying the entire user document.

**Users collection:**

```json
{
  "_id": "507f1f77bcf86cd799439011",
  "username": "john_doe",
  "email": "john@example.com",
  "posts": [
    {
      "content": "Hello, world!",
      "created_at": "2024-01-01T10:00:00Z"
    },
    {
      "content": "My first post",
      "created_at": "2024-01-01T10:05:00Z"
    }
  ],
  "created_at": "2024-01-01T10:00:00Z"
}
```

> [!tip]
> System design interviews intentionally scope functional requirements to a clear, concise set. This means you're unlikely to have "evolving schemas" in the first place, which removes the main reason to choose document databases. Only consider them if your interviewer explicitly mentions rapidly changing data structures.

**When to consider over SQL:** 
- When your schema changes frequently, when you have deeply nested data that would require many joins in SQL, or when different records have vastly different structures.
- A user profile system where some users have extensive work histories while others have minimal data fits this pattern.

**Data modeling impact:** 
- You'll denormalize more aggressively, embedding related data within documents to avoid expensive lookups across collections.
- This trades storage space and update complexity for read performance.

**Example technologies include** MongoDB, Firestore, and CouchDB.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Database Model Options]] — when to pick document over SQL
- [[Relational Databases]] — the SQL alternative
- [[Normalization vs Denormalization]] — document databases favor denormalization
