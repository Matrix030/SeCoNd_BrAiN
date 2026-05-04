In system design, caching comes up almost every time you need to handle high read traffic. Your database becomes the bottleneck, latency starts creeping up, and the interviewer is waiting for you to say the word: **cache.**

1) Reading a user profile from Postgres may take 50 milliseconds, but reading from an in-memory cache like Redis takes just 1 millisecond. 
2) That's a 50x improvement in latency.
3) Databases store data on disk, and every query pays the cost of disk access.
4) Memory sits much closer to the CPU and avoids that entirely.
5) Caches are essential for scalable systems. 
6) They reduce load on the database and cut latency dramatically. 
7) But they also create new challenges around invalidation and failure handling.

## Where to Cache

1) When most engineers hear caching, they immediately think of Redis or Memcached sitting between the application and the database.
2) But caching shows up in multiple layers of a system. 
	- Browsers cache.
	- CDNs cache.
	- Applications cache.
	- Even databases have built-in caching layers.

Let's look at the main places you can cache data, why each one exists, and when it makes sense to use it.

### External Caching

1) An external cache is a standalone cache service that your application talks to over the network. 
2) This is what most people think of when they hear caching.
3) You store frequently accessed data in something like [Redis](https://www.hellointerview.com/learn/system-design/deep-dives/redis) or [Memcached](https://memcached.org/) so you do not have to hit the database every time.

![[Pasted image 20260504165823.png]]

4) External caches scale well because every application server can share the same cache. 
5) They also support eviction policies like LRU and expiration via TTL so your memory footprint stays controlled.

> [!tip]
> In system design, external caching with Redis is the default answer when discussing caching strategies. Interviewers expect you to mention it for any high-traffic system. Start here, then layer on other caching types such as CDN or client-side caching only if the problem calls for them.

