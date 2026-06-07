---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.1.2"]
---

# Benefits

The benefits of multithreaded programming can be broken down into four major categories:
1. **Responsiveness**. 
	- Multithreading an interactive application may allow a program to continue running even if part of it is blocked or is performing a lengthy operation, thereby increasing responsiveness to the user. 
	- For instance, a multithreaded Web browser could allow user interaction in one thread while an image was being loaded in another thread.
2. **Resource sharing**. 
	- Processes may only share resources through techniques such as shared memory or message passing. 
	- Such techniques must be explicitly arranged by the programmer. 
	- However, threads share the memory and the resources of the process to which they belong by default. 
	- The benefit of sharing code and data is that it allows an application to have several different threads of activity within the same address space.
3. **Economy**. 
	- Allocating memory and resources for process creation is costly.
	- Because threads share the resources of the process to which they belong, it is more economical to create and context-switch threads. 
	- Empirically gauging the difference in overhead can be difficult, but in general it is much more time consuming to create and manage processes than threads.

4. **Scalability.** 
	- The benefits of multithreading can be greatly increased in a multiprocessor architecture, where threads may be running in parallel on different processors. A single-threaded process can only run on one processor, regardless how many are available.
	- Multithreading on a multi-CPU machine increases parallelism. We explore this issue further in the following section.

## Related

- [[> Overview]] — back to the sub-topic MOC
- [[Motivation]] — why applications go multithreaded
- [[Multicore Programming]] — the scalability angle, expanded
- [[Shared-Memory Systems]] — the explicit sharing threads avoid
- [[Context Switch]] — the operation threads make cheaper
