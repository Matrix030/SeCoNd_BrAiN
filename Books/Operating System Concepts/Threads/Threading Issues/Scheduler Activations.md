---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.4.6"]
---

# Scheduler Activations

1) A final issue to be considered with multithreaded programs concerns communication between the kernel and the thread library, which may be required by the [[Many-to-Many Model|many-to-many and two-level models]] discussed in Section 4.2.3. Such coordination allows the number of kernel threads to be dynamically adjusted to help ensure the best performance.
2) Many systems implementing either the many-to-many or the two-level model place an intermediate data structure between the user and kernel threads. This data structure—typically known as a **lightweight process**, or LWP—is shown in the figure below. To the user-thread library, the LWP appears to be a _virtual processor_ on which the application can schedule a user thread to run. 
3) Each LWP is attached to a kernel thread, and it is kernel threads that the operating system schedules to run on physical processors. If a kernel thread blocks (such as while waiting for an I/O operation to complete), the LWP blocks as well. Up the chain, the user-level thread attached to the LWP also blocks.

![[Pasted image 20260607122858.png]]

4) An application may require any number of LWPs to run efficiently. Consider a CPU-bound application running on a single processor. In this scenario, only one thread can run at once, so one LWP is sufficient.
5) An application that is I/O-intensive may require multiple LWPs to execute, however. Typically, an LWP is required for each concurrent blocking system call. Suppose, for example, that five different file-read requests occur simultaneously. Five LWPs are needed, because all could be waiting for I/O completion in the kernel. If a process has only four LWPs, then the fifth request must wait for one of the LWPs to return from the kernel.
6) One scheme for communication between the user-thread library and the kernel is known as **scheduler activation**. It works as follows: The kernel provides an application with a set of virtual processors (LWPs), and the application can schedule user threads onto an available virtual processor. Furthermore, the kernel must inform an application about certain events. 
[TODO - wtf upcall]
7) This procedure is known as an **upcall**. 
	- Upcalls are handled by the thread library with an **upcall handler**, and upcall handlers must run on a virtual processor. 
	- One event that triggers an upcall occurs when an application thread is about to block. 
	- In this scenario, the kernel makes an upcall to the application informing it that a thread is about to block and identifying the specific thread. 
	- The kernel then allocates a new virtual processor to the application. The application runs an upcall handler on this new virtual processor, which saves the state of the blocking thread and relinquishes the virtual processor on which the blocking thread is running.
	- The upcall handler then schedules another thread that is eligible to run on the new virtual processor. When the event that the blocking thread was waiting for occurs, the kernel makes another upcall to the thread library informing it that the previously blocked thread is now eligible to run. 
	- The upcall handler for this event also requires a virtual processor, and the kernel may allocate a new virtual processor or preempt one of the user threads and run the upcall handler on its virtual processor. 
	- After marking the unblocked thread as eligible to run, the application schedules an eligible thread to run on an available virtual processor.

## Related

- [[> Threading Issues]] — back to the sub-topic MOC
- [[Thread-Specific Data]] — the previous threading issue
- [[Many-to-Many Model]] — the model that needs this kernel/library coordination
- [[Multicore Programming]] — why scheduling onto multiple processors matters
