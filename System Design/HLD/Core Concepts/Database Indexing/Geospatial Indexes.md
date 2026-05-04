---
tags: [system-design, hld, database-indexing, geospatial]
aliases: ["Geospatial Index", "Spatial Index", "Geohash", "Quadtree", "R-Tree"]
---

# Geospatial Indexes

Here's an interesting quirk of system design interviews:
- while geospatial indexes are fairly specialized in practice - you might never touch them unless you're working with location data - they've become a common focus in interviews.
- Why? The rise of location-based services like Uber, Yelp, and Find My Friends has made proximity search a favorite interview topic.

#### The Challenge with Location Data

1) Say we're building a restaurant discovery app like Yelp.
2) We have millions of restaurants in our database, each with a latitude and longitude. 
3) A user opens the app and wants to find "restaurants within 5 miles of me." Seems simple enough, right?
4) The naive approach would be to use standard [[B-Tree Indexes]] on latitude and longitude:

```
CREATE TABLE restaurants (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    latitude DECIMAL(10, 8),
    longitude DECIMAL(11, 8)
);

CREATE INDEX idx_lat ON restaurants(latitude);
CREATE INDEX idx_lng ON restaurants(longitude);
```

5) But this falls apart quickly when we try to execute a proximity search.
6) Think about how a [[B-Tree Indexes|B-tree index]] on latitude and longitude actually works. 
7) We're essentially trying to solve a 2D spatial problem (finding points within a circle) using two separate 1D indexes.
8) When we query "restaurants within 5 miles," we'll inevitably hit one of two performance problems:
	- If we use the latitude index first, we'll find all restaurants in the right latitude range - but that's a long strip spanning the entire globe at that latitude!
	- Then for each of those restaurants, we need to check if they're also in the right longitude range.
	- Our index on longitude isn't helping because we're not doing a range scan - we're doing point lookups for each restaurant we found in the latitude range.
	- If we try to be clever and use both indexes together (via an index intersection), the database still has to merge two large sets of results - all restaurants in the right latitude range and all restaurants in the right longitude range. 
	- This creates a rectangular search area much larger than our actual circular search radius, and we still need to filter out results that are too far away.

![[Pasted image 20260503190442.png]]

9) This is why we need indexes that understand 2D spatial relationships. Rather than treating latitude and longitude as independent dimensions, spatial indexes let us organize points based on their actual proximity in 2D space.

#### Core Approaches

1) There are three main approaches you'll encounter in interviews: geohashes, quadtrees, and R-trees.
2) Each has its own strengths and trade-offs, but all solve our fundamental problem: they preserve spatial relationships in our index structure.
3) While this seems like a lot of specialized knowledge, mainly want to see that you understand the basic problem (why regular indexes fall short) and can reason about at least one solution.
4) You don't need deep expertise in all three approaches unless you're interviewing for a role that specifically works with location data.

#### Geohash

We'll start with geohash because it's the simplest spatial index to understand and the core idea is beautifully simple: convert a 2D location into a 1D string in a way that preserves proximity.

![[Pasted image 20260503191403.png]]

1) Imagine dividing the world into four squares and labeling them 0-3.
2) Then divide each of those squares into four smaller squares, and so on. 
3) Each division adds more precision to our location description. 
4) A geohash is essentially this process, but using a base32 encoding that creates strings like "dr5ru" for locations. 
5) The longer the string, the more precise the location:
- "9q8y" might represent all of San Francisco
- "9q8yy" narrows it down to the Mission District
- "9q8yyk" might pinpoint a specific city block

6) What makes geohash clever is that locations that are close to each other usually share similar prefix strings.
7) Two restaurants on the same block might have geohashes that start with "9q8yyk", while a restaurant in a different neighborhood might start with "9q8yym".
8) And here's the real elegance: once we've converted our 2D locations into these ordered strings, we can use a regular [[B-Tree Indexes|B-tree index]] to handle our spatial queries. 
9) Remember how [[B-Tree Indexes|B-trees]] excel at prefix matching and range queries? That's exactly what we need for proximity searches.

When we index the geohash strings with a B-tree:

```
CREATE INDEX idx_geohash ON restaurants(geohash);
```

10) Finding nearby locations becomes a matter of searching strings with matching prefixes. 
11) If we're looking for restaurants near geohash "9q8yyk", we can do a range scan in our B-tree for entries starting with that prefix. 
12) For radius queries, we also need to include adjacent geohash cells since our search area might span cell boundaries - but this is still just a handful of prefix range scans.
13) This lets us leverage all the optimizations that databases already have for B-trees - no special spatial data structure needed.

This is why Redis's geospatial commands use this approach internally. When you run:

```
GEOADD restaurants -122.4194 37.7749 "Restaurant A"
GEOSEARCH restaurants FROMLONLAT -122.4194 37.7749 BYRADIUS 5 mi
```

> [!info]
> GEORADIUS is deprecated in favor of GEOSEARCH. Redis uses geohash under the hood to efficiently find nearby points.

1) The main limitation of geohash is that locations near each other in reality might not share similar prefixes if they happen to fall on different sides of a major grid division - like two restaurants on opposite sides of a street that marks a geohash boundary. 
2) But for most applications, this edge case isn't significant enough to matter.
3) This elegance - turning a complex 2D problem into simple string prefix matching that can leverage existing B-tree implementations - is why geohash is such a popular choice. 
4) It's easy to understand, implement, and use with existing database systems that already know how to handle strings efficiently.

#### Quadtree

1) While less common in production databases today, quadtrees represent a fundamental tree-based approach to indexing 2D space that has shaped how we think about spatial indexing.
2) Unlike geohash which transforms coordinates into strings, quadtrees directly partition space by recursively subdividing regions into four quadrants.
3) Start with one square covering your entire area. When a square contains more than some threshold of points (typically 4-8), split it into four equal quadrants.
4) Continue this recursive subdivision until reaching a maximum depth or achieving the desired point density per node.
5) This spatial partitioning maps to a tree structure:

![[Pasted image 20260503200201.png]]

6) For proximity searches, navigate down the tree to find the target quadrant, check neighboring quadrants at the same level, and adjust the search radius by moving up or down tree levels as needed.
7) The **key advantage** of quadtrees is their adaptive resolution - dense areas get subdivided more finely while sparse regions maintain larger quadrants.
8) However, unlike geohash which leverages existing B-tree implementations, quadtrees require specialized tree structures. 
9) This implementation complexity explains why most modern databases prefer geohash or R-tree variants.
10) So while they're not common in production nowadays, quadtrees have a significant impact on modern spatial indexing. 
11) The core concept of recursive spatial subdivision forms the foundation for R-trees, which optimize these ideas for disk-based storage and better handling of overlapping regions.
12) You'll even find quadtree principles in modern mapping systems - Google Maps uses a similar structure for organizing map tiles at different zoom levels.

#### R-Tree
1) R-trees have emerged as the default spatial index in modern databases like PostgreSQL/PostGIS and MySQL. 
2) While both quadtrees and R-trees organize spatial data hierarchically, R-trees take a fundamentally different approach to how they divide space.
3) Instead of splitting space into fixed quadrants, R-trees work with flexible, overlapping rectangles.
4) Where a quadtree rigidly divides each region into four equal parts regardless of data distribution, R-trees adapt their rectangles to the actual data. 
5) Think of organizing photos on a table - a quadtree approach would divide the table into equal quarters and keep subdividing, while an R-tree would let you create natural, overlapping groupings of nearby photos.
![[Pasted image 20260504135437.png]]

6) When searching for nearby restaurants in San Francisco, an R-tree might first identify the large rectangle containing the city, then drill down through progressively smaller, overlapping rectangles until reaching individual restaurant locations.
7) These rectangles aren't constrained to fixed sizes or positions - they adapt to wherever your data actually clusters.
8) A quadtree, in contrast, would force you to navigate through a rigid grid of increasingly smaller squares, potentially requiring more steps to reach the same destinations.
9) This flexibility offers a crucial advantage: R-trees can efficiently handle both points and larger shapes in the same index structure.
10) A single R-tree can index everything from individual restaurant locations to delivery zone polygons and road networks.
11) The rectangles simply adjust their size to bound whatever shapes they contain.
12) Quadtrees struggle with this mixed data - they keep subdividing until they can isolate each shape, leading to deeper trees and more complex traversal.
13) The trade-off for this flexibility is that overlapping rectangles sometimes force us to search multiple branches of the tree.
14) Modern R-tree implementations use smart algorithms to balance this overlap against tree depth, tuning for how databases actually read data from disk.
15) This balance of flexibility and disk efficiency is why R-trees have become the standard choice for production spatial indexes.

> [!tip]
> focus on explaining the problem clearly and contrasting a tree-based approach with a hash-based approach.
> 
> For example, you could say something like:
> 
> "Traditional indexes like B-trees don't work well for spatial data because they treat latitude and longitude as independent dimensions. To efficiently search for nearby locations, we need an index that understands spatial relationships. Geohash is a hash-based approach that converts 2D coordinates into a 1D string, preserving proximity. This allows us to use a regular B-tree index on the geohash strings for efficient proximity searches. However, tree-based approaches like R-trees can offer more flexibility and accuracy by grouping nearby objects into overlapping rectangles, creating a hierarchy of bounding boxes."
> 
> By contrasting these two approaches, you demonstrate a deeper understanding of the trade-offs involved in geospatial indexing.

## Related

- [[> Database Indexing]] — back to the section MOC
- [[B-Tree Indexes]] — the underlying structure geohash leverages for prefix queries
- [[Inverted Indexes]] — another specialized index for non-standard query types
