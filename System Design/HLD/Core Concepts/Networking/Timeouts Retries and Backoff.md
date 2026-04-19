---
tags: [system-design, hld, networking, fault-tolerance, reliability]
aliases: ["Retries", "Exponential Backoff", "Idempotency", "Idempotency Key", "Backoff"]
---

# Timeouts, Retries, and Backoff

### Handling Failures and Fault Modes

Part of this is server failures:
- servers crash
- solar flares can flip bits
- power can be cut.

But we may also deal with network failures!
- Cables get cut
- routers fail
- packets get dropped

Robust system design requires planning for these failures.

> [!tip]
> The fallacy of "the network is reliable" is one of the most dangerous assumptions in distributed systems. Always design with the expectation that network calls will fail, be delayed, or return unexpected results.


#### Timeouts and Retries with Backoff

1) The most elementary hygiene for handling failures is to use timeouts and retries.
2) If we expect a request to take a certain amount of time, we can set a timeout and if the request takes too long we can give up and try again.
3) Retrying requests is a great strategy for dealing with transient failures.
4) If a server is temporarily slow, we can retry the request and it will likely succeed.
5) Having **idempotent APIs** is key here because we can retry the same request multiple times without causing issues.

##### Backoff

1) Retries can be a double-edged sword, though.
2) If we have a lot of retries, we may be retrying requests that are going to fail over and over again.
3) This is why most retry strategies also include a **backoff** strategy.
4) Instead of retrying immediately, we wait a short amount of time before retrying.
5) If the request still fails, we wait a little longer.
6) This gives the system time to recover and reduces the load on the system.
7) It's important there is some randomness to the backoff strategy (often called **"jitter"**).
8) It doesn't help us to have all of our clients retry at the same time! The worst case would be having all our failing requests synchronize and retry at the same time over and over again like a jackhammer. No good.

In system design interviews, **interviewers are often looking for the magic phrase "retry with exponential backoff"**. In more senior interviews, you may be asked to elaborate about adding jitter. Pair this with [[Idempotency]] to avoid side effects and [[Circuit Breakers]] to prevent cascading failures.

AWS has a great blog post on the [timeouts, retries, and backoff](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/) if you want to learn more.

##### Idempotency

1) Imagine a payment system where we're trying to charge a user $10 for something. If we retry the same request multiple times, we're going to charge the user $20 (or $2,000) instead of $10.
2) This is why we need to make sure our APIs are **idempotent**.
3) Idempotent APIs are APIs that can be called multiple times and they produce the same result every time.
4) HTTP GET requests are common examples of idempotent APIs.
5) While the content returned by a GET request may change, the act of fetching the content does not change the state of the system.

But reading data is easy, how about writing data?
- For these cases, it's common for us to introduce an **idempotency key** to our API.
- The idempotency key is a unique identifier for a request that we can use to make sure the same request is idempotent.

1) For our payment example, if we know a user is only ever going to buy one item per day, we can set an idempotency to the user's ID and the current date.
2) On the server-side, we can check to see if we've already processed (or are currently processing) a request with that idempotency key and process it only once.
3) User-friendly APIs will wait for the request to complete then send the results to all requesters.
4) Less friendly APIs will just return an error saying the request already exists.
5) But both will keep you from double charging your user's credit cards.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[Circuit Breakers]] — the next level of protection against cascading failures
- [[Load Balancing]] — health checks complement retry logic
