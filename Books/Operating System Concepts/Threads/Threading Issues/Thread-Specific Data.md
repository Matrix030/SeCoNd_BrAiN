---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.4.5"]
---

# Thread-Specific Data

1) Threads belonging to a process share the data of the process. Indeed, this sharing of data provides one of the benefits of multithreaded programming.
2) However, in some circumstances, each thread might need its own copy of certain data. We will call such data **thread-specific data**. For example, in a transaction-processing system, we might service each transaction in a separate thread. Furthermore, each transaction might be assigned a unique identifier. To associate each thread with its unique identifier, we could use thread-specific data. 
3) Most thread libraries—including Win32 and Pthreads—provide some form of support for thread-specific data. Java provides support as well.

## Related

- [[> Threading Issues]] — back to the sub-topic MOC
- [[Thread Pools]] — the previous threading issue
- [[Scheduler Activations]] — the next threading issue
- [[Thread Library Implementation]] — the libraries that support thread-specific data
