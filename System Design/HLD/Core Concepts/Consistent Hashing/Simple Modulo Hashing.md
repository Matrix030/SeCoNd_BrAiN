---
tags: [system-design, hld, scaling]
aliases: ["Modulo Hashing"]
---

# First Attempt: Simple Modulo Hashing

1) Imagine you're designing a ticketing system like TicketMaster. 
2) Initially, your system is simple:
	- One database storing all event data
	- Clients making requests to fetch event information
	- Everything works smoothly at first
![[Pasted image 20260505151812.png]]

3) As your platform grows popular and hosts more events, a single database can no longer handle the load. You need to distribute your data across multiple databases – a process called [[Sharding]].
![[Pasted image 20260505155211.png]]

The question we need to answer is: **How do we know which events to store on which database instance?**

The most straightforward approach to distribute data across multiple databases is modulo hashing.
1. First, we take the event ID and run it through a hash function, which converts it into a number
2. Then, we take that number and perform the modulo operation (%) with the number of databases
3. The result tells us which database should store that event
![[Pasted image 20260505155310.png]]

4) In code, it looks like this:

```
database_id = hash(event_id) % number_of_databases
```

5) For a concrete example with 3 databases:

```
Event #1234 → hash(1234) % 3 = 1 → Database 1
Event #5678 → hash(5678) % 3 = 0 → Database 0
Event #9012 → hash(9012) % 3 = 2 → Database 2
```

6) Great! This works well, until you run into a few problems.
	- The first problem comes when you want to add a fourth database instance. To do so, you naively think that all you need to do is run the modulo operation with 4 instead of 3.

```
database_id = hash(event_id) % 4
```

- You change the code, and push to production but then your database activity goes through the roof! Not just for the fourth database instance either, but for all of them.
- What happened was the change in the hash function did not only impact data that should be stored on the new instance, but it changed which database instance almost _every_ event was stored on.

![[Pasted image 20260505160141.png]]

7) For example, event #1234 used to map to database 1, but now, hash(1234) % 4 = 0 so that data instead needs to be moved to database 0.
8) This means the data needs to be moved from database 1 to database 0.
9) This isn't an isolated case - most of your data needs to be redistributed across all database instances, causing massive unnecessary data movement. This causes huge spikes in database load and means users are either unable to access data or they experience slow response times.

Let's look at another problem with simple modulo hashing.
	- Imagine a database went down.
	- This could be due to anything from a hardware failure to a software bug. 
	- In any case, we need to remove it from the pool of databases and redistribute the data across the remaining instances until we can pull a new one online.
	- Our hash function now changes from database_id = hash(event_id) % 3 to database_id = hash(event_id) % 2 and we run into the exact same redistribution problem we had before.

![[Pasted image 20260505160441.png]]

Clearly simple modulo hashing isn't cutting it. Enter, **[[Consistent Hashing]]**.

## Related

- [[> Consistent Hashing]] — back to the section MOC
- [[The Hash Ring]] — how consistent hashing solves the redistribution problem
- [[Sharding]] — the broader concept of distributing data across multiple databases
