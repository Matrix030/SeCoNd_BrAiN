---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.6.2"]
---

# Mechanisms and Policies

1) One important principle is the separation of **policy** from **mechanism**. 
	- Mechanisms determine **_how_** to do something
	- policies determine **_what_** will be done. 
2) For example, the timer construct is a mechanism for ensuring CPU protection, but deciding how long the timer is to be set for a particular user is a policy decision.
3) The separation of policy and mechanism is important for flexibility. Policies are likely to change across places or over time. In the worst case, each change in policy would require a change in the underlying mechanism. A general mechanism insensitive to changes in policy would be more desirable. 
4) A change in policy would then require redefinition of only certain parameters of the system. For instance, consider a mechanism for giving priority to certain types of programs over others. If the mechanism is properly separated from policy, it can be used either to support a policy decision that I/O-intensive programs should have priority over CPU-intensive ones or to support the opposite policy.
5) Microkernel-based operating systems take the separation of mechanism and policy to one extreme by implementing a basic set of primitive building blocks. 
6) These blocks are almost policy free, allowing more advanced mechanisms and policies to be added via user-created [[kernel]] modules or via user programs themselves. As an example, consider the history of UNIX. At first, it had a time-sharing scheduler.
7) Policy decisions are important for all resource allocation. Whenever it is necessary to decide whether or not to allocate a resource, a policy decision must be made. Whenever the question is _how_ rather than _what,_ it is a mechanism that must be determined.

## Related

- [[> Operating-System Design and Implementation]] — back to the sub-topic MOC
- [[Design Goals]] — the previous design concern
- [[Implementation]] — the next design concern
