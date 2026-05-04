---
tags: [system-design, hld, data-modeling]
aliases: ["Database Models"]
---

# Database Model Options

1) Before you can design a schema, you need to pick what type of database you're working with.
2) Different database models shape how you structure your data, so this choice affects everything that follows.
3) Most of the time, the right answer is a relational database. It's the default unless your requirements clearly signal a specialized model.
4) Unless you have significant experience and, with it, strong opinions about another database, it is recommended to stick with postgreSQL.
5) That doesn't mean other database models aren't worth knowing. 
6) Showing you understand when they might be useful demonstrates that you're thinking about trade-offs, not just parroting the default. 
7) Still, the star of the show is SQL, so we'll start there before briefly touching on the alternatives.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Relational Databases]] — the default choice: SQL and ACID
- [[Document Databases]] — flexible schemas, JSON documents
- [[Key-Value Stores]] — fast lookups, caching
- [[Wide-Column Databases]] — write-heavy, time-series
- [[Graph Databases]] — nodes and edges
