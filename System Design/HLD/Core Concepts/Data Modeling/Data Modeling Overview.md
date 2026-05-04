---
tags: [system-design, hld, data-modeling]
aliases: ["Data Modeling Intro"]
---

# Data Modeling Overview

1) Data modeling is the process of defining how your application's data is structured, stored, and related. 
2) In practice, this means deciding what entities exist, how they're identified, and how they connect to one another.
3) You're not expected to normalize everything or produce a complete schema diagram, you're just expected to design something clear, functional, and aligned with your system's requirements.
4) In the [[Delivery Framework]]. These usually map 1:1 with tables or collections and form the backbone of your schema. Later, in the High Level Design {TODO: connect this with High level design header in [[Delivery Framework]]}, you'll sketch a basic schema alongside your database component. 
5) Include the key fields, relationships, and a note on how you'd index or partition to support the main query patterns.

![[Pasted image 20260424124945.png]]

6) A reasonable schema sets up the rest of your design, such as scaling reads and writes, preserving consistency when it matters, and answering questions about growth or auditability without backtracking.

## Related

- [[> Data Modeling]] — back to the section MOC
- [[Database Model Options]] — picking the right database type
- [[Schema Design Fundamentals]] — how requirements drive schema design
