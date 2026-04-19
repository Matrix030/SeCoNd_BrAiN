---
tags: [system-design, hld, networking, scaling, cdn, latency]
aliases: ["CDN", "Content Delivery Network", "Regional Partitioning", "Data Locality"]
---

# Regionalization and Latency

1) For global services, you're typically going to have servers distributed across the world.
2) A common pattern is to have multiple data centers in a single region (Amazon calls these "availability zones") so that e.g. a pipe breakage in one building doesn't take down your whole service, and then replicate this model across multiple cities spread across the world.
3) But while this global deployment is a great, it does introduce new networking challenges. The physical distance between clients and servers significantly impacts network latency.
4) Speed of light limitations mean that a request from New York to London will always have higher latency than a request to a nearby server (<1ms vs >80ms).

> [!info]
> Light travels through fiber optic cables at about 2/3 the speed of light in a vacuum, which is approximately 200,000 km/s. This means a round trip between New York and London (about 5,600 km) has a theoretical minimum latency of around 56ms just from the physics of signal propagation, before adding any processing time. This physical constraint is why geographic distribution is essential for low-latency applications.

What do we do about this?

1) In order to address this problem, we need to return to **data locality**. Across all of computing, we're going to have highest performance when the data is as close as possible to the computations we need to do.
2) For a regional application, we want to try to keep all of the data we need to satisfy a query:
	a) as close together
	b) as close to the user as possible.

	If our user data is in Los Angeles, but our web servers are in New York, every database query will have tens of milliseconds of network-induced latency. And that's before we even consider the processing time of the results!

Some of this latency is **unavoidable**. If our users are simply far apart, there's nothing we can actually do to change that. But there are a couple of strategies we can use to optimize within the constraints of physics.

#### Content Delivery Networks (CDNs)

1) The most common strategy for reducing latency is to use a **Content Delivery Network (CDN)**.
2) CDNs are networks of servers that are strategically located around the world.
3) CDNs frequently boast hundreds or even thousands of different cities where they have servers.
4) These servers make up what is commonly referred to as an **"edge location"**.
5) If that edge server can answer a user's request, the user is going to get lightning fast response times — the data is just up the road!
6) This is only possible because of _caching_. If our data doesn't change a lot, or doesn't need to be updated frequently, we can cache it at the edge server and return it from there. This is especially effective for static content like images, videos, and other assets.
7) Using a CDN as a cache for e.g. [search results on Facebook](https://www.hellointerview.com/learn/system-design/problem-breakdowns/fb-post-search#1-how-can-we-handle-the-large-volume-of-requests-from-users) allows us to both minimize latency **and** reduce the load on our backend servers.

#### Regional Partitioning

1) If we have a lot of users in a single region, we can partition our data by region so that each region only has data relevant to it.

Let's take Uber as an example.
- With the Uber app we're ordering rides from drivers in a specific city.
- If we're in Miami, we'll never want to book a ride with a driver currently in New York. This is an important insight!
- While on any given day we may have millions of riders and drivers, inside one particular city we may only have a few thousand. Our physical architecture and network topology can mirror this!
- We can bundle together nearby cities into a single local region (e.g. "Northeast US", or "Southwest US").
- Each region can have its own database hosted on distinct servers located in that geography (maybe we put our data centers in New York and Los Angeles).
- The servers handling requests can be co-located alongside the databases they need to query.
- Then when users want to book a ride, or look up their status, their queries can be answered by their regional services (fast), and those regional services can use a local database to process the query (very fast).

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[Load Balancing]] — pair with CDNs and regional routing
- [[Circuit Breakers]] — fault tolerance when regional services fail
