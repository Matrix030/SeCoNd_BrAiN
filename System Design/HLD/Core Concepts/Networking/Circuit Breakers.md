---
tags: [system-design, hld, networking, fault-tolerance, reliability]
aliases: ["Circuit Breaker Pattern", "Cascading Failures"]
---

# Circuit Breakers

1) The last topic we see commonly in deep dives is how to handle **cascading failures** in a system.
2) Senior candidates are frequently asked questions like "what happens when this service goes down".
3) Sometimes the answer is simple: "we fail and retry until it boots back up" — but occasionally that will introduce new problems for the system!
4) If your database has gone down cold and you need to boot it up one instance at a time, having a firehose of retries and angry users might pin down an instance from ever getting started (sometimes ominously referred to as a "thundering herd").
5) You can't get the first instance up, so you have no hope of getting the whole database back online. You're stuck!

> [!tip]
> Experienced engineers who have spent time oncall will have a lot of war stories about cascading failures. It's a common problem that usually goes unnoticed until it bites you at 3am.
>
> As such, it makes for a great interview question! Not only does it help you to find candidates who understand how to prevent some of the most pernicious issues, but it's also a decent screen for experience which many interviewers are looking for.
>
> The key for your preparation is to familiarize yourself with scenarios where one failure might create new failures: a cascade of failures. Being able to identify these patterns and how to mitigate them is a great way to stand out in an interview.

Enter circuit breakers: a crucial pattern for robust system design that directly impacts network communication. Circuit breakers protect your system when network calls to dependencies fail repeatedly. Here's how they work:

1. The circuit breaker monitors for failures when calling external services
2. When failures exceed a threshold, the circuit "trips" to an open state
3. While open, requests immediately fail without attempting the actual call
4. After a timeout period, the circuit transitions to a "half-open" state
5. A test request determines whether to close the circuit or keep it open

This pattern, inspired by electrical circuit breakers, prevents cascading failures across distributed systems and gives failing services time to recover.

Circuit breakers provide numerous advantages:

- Fail Fast: Quickly reject requests to failing services instead of waiting for timeouts
- Reduce Load: Prevent overwhelming already struggling services with more requests
- Self-Healing: Automatically test recovery without full traffic load
- Improved User Experience: Provide fast fallbacks instead of hanging UI
- System Stability: Prevent failures in one service from affecting the entire system

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[Timeouts Retries and Backoff]] — retries + backoff that circuit breakers complement
- [[Load Balancing]] — health checks as a simpler first line of defense
- [[Regionalization and Latency]] — regional failures as a common circuit breaker trigger
