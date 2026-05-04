---
tags: [system-design, hld, data-modeling]
aliases: ["Graph Database", "Neo4j"]
---

# Graph Databases

Graph databases store data as nodes and edges, optimizing for traversing relationships between entities.

![[Pasted image 20260424132112.png]]

**When to consider over SQL:** 
- Honestly? Almost never in interviews.
- The classic examples are social networks and recommendation engines, but even Facebook models their social graph with MySQL. 
- If it's good enough for the world's largest social network, it's probably good enough for your interview.

**Example technologies include** Neo4j and Amazon Neptune.

> [!warning]
> They sound sophisticated but add unnecessary complexity. Even "graph-heavy" companies like LinkedIn and Twitter use SQL for their core relationship data. Other databases can handle the primary query patterns without the operational complexity that comes with specialized graph systems.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Database Model Options]] — why graph databases are rarely the right call
- [[Relational Databases]] — SQL handles most "graph" use cases in practice
