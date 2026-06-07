---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.2"]
---

# User and Kernel Threads

Our discussion so far has treated threads in a generic sense. However, support for threads may be provided either at the user level, for **user threads**, or by the kernel, for **kernel threads**. 

User threads are supported above the kernel and are managed without kernel support, whereas kernel threads are supported and managed directly by the operating system.

Ultimately, a relationship must exist between user threads and kernel threads. In this section, we look at three common ways of establishing such a relationship.

## Related

- [[> Multithreading Models]] — back to the sub-topic MOC
- [[Many-to-One Model]] — the first relationship
- [[One-to-One Model]] — the second relationship
- [[Many-to-Many Model]] — the third relationship
