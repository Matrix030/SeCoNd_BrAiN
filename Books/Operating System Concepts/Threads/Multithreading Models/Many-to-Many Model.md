---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.2.3"]
---

# Many-to-Many Model

1) The many-to-many model multiplexes many user-level threads to a smaller or equal number of kernel threads. The number of kernel threads may be specific to either a particular application or a particular machine (an application may be allocated more kernel threads on a multiprocessor than on a uniprocessor). 
2) Whereas the many-to-one model allows the developer to create as many user threads as she wishes, true concurrency is not gained because the kernel can schedule only one thread at a time. 
3) The one-to-one model allows for greater concurrency, but the developer has to be careful not to create too many threads within an application (and in some instances may be limited in the number of threads she can create). 
4) The many-to-many model suffers from neither of these shortcomings: developers can create as many user threads as necessary, and the corresponding kernel threads can run in parallel on a multiprocessor. 
5) Also, when a thread performs a blocking system call, the kernel can schedule another thread for execution.
6) One popular variation on the many-to-many model still multiplexes many user-level threads to a smaller or equal number of kernel threads but also allows a user-level thread to be bound to a kernel thread. 


![[Pasted image 20260607111004.png]]

7) This variation, sometimes referred to as the _two-level model_ (figure below), is supported by operating systems such as IRIX, HP-UX, and Tru64 UNIX.

![[Pasted image 20260607111014.png]]

## Related

- [[> Multithreading Models]] — back to the sub-topic MOC
- [[Many-to-One Model]] — the model whose concurrency limit this fixes
- [[One-to-One Model]] — the model whose thread-count limit this fixes
- [[Multiprocessor Systems]] — where the kernel threads run in parallel
