---
tags: [system-design, hld, scaling, moc]
aliases: ["Consistent Hashing"]
---

# Consistent Hashing — Map of Content

> [!info] About this section
> How consistent hashing distributes data across a cluster while minimizing redistribution when nodes are added or removed.

---

## The Problem
- [[Simple Modulo Hashing]] — why naive modulo-based distribution breaks when the cluster size changes

## The Algorithm
- [[The Hash Ring]] — the hash ring, clockwise lookup, and virtual nodes for balanced load distribution
- [[Addressing Hot Spots]] — why even key distribution doesn't prevent traffic hot spots, and how to fix it
- [[Data Movement in Practice]] — what actually happens to data when nodes join or leave

## In Practice
- [[Consistent Hashing in the Real World]] — Cassandra, DynamoDB, CDNs, and how real systems use it
- [[When to Use Consistent Hashing]] — when to mention it in interviews vs. when to let the database handle it

> [!tip] Interview mental model
> Explain simple modulo hashing → show why it fails at scale → introduce the ring → add virtual nodes → address hot spots with replication or key salting.
