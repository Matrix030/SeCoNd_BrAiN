---
tags: [system-design, hld, real-time]
aliases: ["Pulling with Simple Polling", "Pull-Based Polling"]
---

# Pulling with Simple Polling
1) With Simple Polling, we're using a **pull-based** model.
2) Our client is constantly asking the server for updates and the server needs to maintain the state necessary to respond to those requests. 
3) The most common way to do this is to have a database where we can store the updates (e.g. all of the messages in the chat room), and from this database our clients can pull the updates they need when they can.
4) For our chat app, we'd basically be polling for "what messages have been sent to the room with a timestamp larger than the last message I received?".
![[Pasted image 20260524193828.png]]

5) We use the poll itself as the trigger, even though the actual update may have occurred some time prior.
6) The nice thing about this from a system design perspective is that we've _decoupled_ the source of the update from the client receiving it. 
7) The line that receives the updates is interrupted (by the DB) from the line that produces them - data is not _flowing_ from the producer to the consumer. 
8) The downside is that we've lost the real-time aspect of our updates.
## Advantages
- Dead simple to implement.
- State is constrained to our DB.
- No special infrastructure.
- Doesn't take much time to explain.

## Disadvantages
- High latency.
- Excess DB load when updates are infrequent and polling is frequent.

## When to Use Pull-Based Polling
1) Pull-based polling is great when you want your user experience to be somewhat more responsive to changes that happen on the backend, but responding quickly is not the main thing.
2) Generally speaking, if you need real-time updates this is not the best approach, but again there are a lot of use-cases where real-time updates are actually not required!

## Things to Discuss in Your Interview
1) When you're using Pull-based Polling, you'll want to talk about how you're storing the updates.
2) If you're using a database, you'll want to discuss how you're querying for the updates and how that might change given your load.

In many instances where this approach might be used, the number of clients can actually be quite large. If you have a million clients polling every 10 seconds, you've got 100k TPS of read volume! This is easy to forget about.

## Related

- [[> Real-time Updates]] — back to the section MOC
- [[Simple Polling]] — the matching client-side (first-hop) protocol
- [[Pushing via Pub-Sub]] — the push-based alternative when you need real-time
- [[Server-Side Push and Pull]] — the three server-side trigger patterns
