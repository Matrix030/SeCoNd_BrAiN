---
tags: [system-design, hld, caching, scaling]
aliases: ["Cache Layers", "Cache Types"]
---

# Where to Cache

In system design, caching comes up almost every time you need to handle high read traffic. Your database becomes the bottleneck, latency starts creeping up, and the interviewer is waiting for you to say the word: **cache.**

1) Reading a user profile from Postgres may take 50 milliseconds, but reading from an in-memory cache like [[Redis]] takes just 1 millisecond. 
2) That's a 50x improvement in latency.
3) Databases store data on disk, and every query pays the cost of disk access.
4) Memory sits much closer to the CPU and avoids that entirely.
5) Caches are essential for scalable systems. 
6) They reduce load on the database and cut latency dramatically. 
7) But they also create new challenges around invalidation and failure handling.

1) When most engineers hear caching, they immediately think of [[Redis]] or Memcached sitting between the application and the database.
2) But caching shows up in multiple layers of a system. 
	- Browsers cache.
	- CDNs cache.
	- Applications cache.
	- Even databases have built-in caching layers.

Let's look at the main places you can cache data, why each one exists, and when it makes sense to use it.

## External Caching

1) An external cache is a standalone cache service that your application talks to over the network. 
2) This is what most people think of when they hear caching.
3) You store frequently accessed data in something like [Redis](https://www.hellointerview.com/learn/system-design/deep-dives/redis) or [Memcached](https://memcached.org/) so you do not have to hit the database every time.

![[Pasted image 20260504165823.png]]

4) External caches scale well because every application server can share the same cache. 
5) They also support eviction policies like [[LRU]] and expiration via [[TTL]] so your memory footprint stays controlled.

> [!tip]
> In system design, external caching with Redis is the default answer when discussing caching strategies. you are expected to mention it for any high-traffic system. Start here, then layer on other caching types such as CDN or client-side caching only if the problem calls for them.

## CDN (Content Delivery Network)

1) A CDN is a geographically distributed network of servers that caches content close to users. 
2) Instead of every request traveling to your origin server, a CDN stores copies of your content at edge servers around the world.
> [!info]
> Modern CDNs like Cloudflare, Fastly, and Akamai can cache much more than static files. They can also cache public API responses, HTML pages, and even run edge logic to personalize content or enforce security rules before requests reach your servers. But the most common and most impactful use of a CDN is still media delivery.

How it works:

1. A user requests an image from your app.
2. The request goes to the nearest CDN edge server.
3. If the image is cached there, it is returned immediately.
4. If not, the CDN fetches it from your origin server, stores it, and returns it.
5. Future users in that region get the image instantly from the CDN.

![[Pasted image 20260504170317.png]]

6) Without a CDN, every image request travels to your origin.
7) If your server is in Virginia and the user is in India, that adds 250–300 ms of latency per request.
8) With a CDN, the same image is served from a nearby edge server in 20–40 ms. That is a massive performance difference.

> [!tip]
> Even though modern CDNs can cache API responses and dynamic content, in system design interviews the safest time to introduce a CDN is when your system serves static media at scale. Start with that reason first, then expand only if the problem calls for more.

## Client-Side Caching

1) Client-side caching stores data close to the requester to avoid unnecessary network calls.
2) This usually means the user's device, like a browser (HTTP cache, localStorage) or mobile app using local memory or on-device storage.
3) But it can also mean caching within a client library. 
4) For example, Redis clients cache cluster metadata like which nodes are in the cluster and which slots are assigned to them. 
5) That way, the client can route requests directly to the right node without querying the cluster on every operation.
6) For user-facing caching, you have limited control from the backend. Data can go stale and invalidation is harder. 
7) The [Strava app](https://www.hellointerview.com/learn/system-design/problem-breakdowns/strava) keeps your run data on the device while you are offline and syncs it later. A browser reusing a previously downloaded image from disk is also caching.

![[Pasted image 20260504170547.png]]

## In-Process Caching

1) Most candidates, and engineers, overlook the fact that servers run on machines with a lot of memory. As hardware improves, this becomes increasingly true.
2) You can use that memory to cache data directly inside the application process instead of always calling out to Redis or the database.
3) The idea is simple: if your service keeps requesting the same small pieces of data again and again, store them in a local cache inside the process. 
4) Reads from local memory are even faster than reads from Redis because they avoid any network call.
5) This light-weight form of caching makes sense for small pieces of data that are requested frequently like:

- Configuration values
- Feature flags
- Small reference datasets
- Hot keys
- Rate limiting counters
- Precomputed values

![[Pasted image 20260504170726.png]]

6) In-process caching is blazing fast, but it comes with obvious limitations. 
7) Each instance of your application has its own cache, so cached data is not shared across servers.
8) If one instance updates or invalidates a cached value, the others will not know.

> [!tip]
> Use in-process caching for small, frequently accessed values that rarely change. It is great for speed but not a replacement for Redis. In system design interviews, mention this only as an **optimization layer** after you have already introduced an external cache.

## Related

- [[> Caching]] — back to the Caching MOC
- [[Cache Architectures]] — how to read from and write to the cache
- [[Cache Eviction Policies]] — LRU, LFU, FIFO, and TTL
- [[Common Caching Problems]] — cache stampede, consistency issues, and hot keys
