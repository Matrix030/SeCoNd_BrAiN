---
tags: [system-design, hld, data-modeling, moc]
aliases: ["Data Modeling MOC"]
---

# Data Modeling — Map of Content

> [!info] About this section
> Data modeling covers how to structure, store, and relate your application's data — from picking the right database model to designing schemas that support your access patterns.

---

## Overview
- [[Data Modeling Overview]] — what data modeling is and where it fits in the design process
- [[Database Model Options]] — how to choose between relational, document, key-value, wide-column, and graph databases

## Database Models
- [[Relational Databases]] — SQL, ACID guarantees, the default for most systems
- [[Document Databases]] — flexible schemas, embedded documents, MongoDB/Firestore
- [[Key-Value Stores]] — simple lookups, caching, Redis/DynamoDB
- [[Wide-Column Databases]] — write-heavy workloads, time-series, Cassandra/HBase
- [[Graph Databases]] — nodes and edges, rarely the right choice in interviews

## Schema Design
- [[Schema Design Fundamentals]] — requirements-driven schema design: data volume, access patterns, consistency
- [[Entities Keys and Relationships]] — primary keys, foreign keys, 1:N and N:M relationships
- [[Indexing for Access Patterns]] — indexing strategy tied to your API endpoints
- [[Normalization vs Denormalization]] — when to keep data clean, when to duplicate for performance
- [[Scaling and Sharding]] — partitioning data across machines, choosing a shard key

> [!tip] Interview mental model
> Default to SQL with a normalized schema. Denormalize only when you can justify it against access patterns. Shard by your primary query key and avoid cross-shard queries.
