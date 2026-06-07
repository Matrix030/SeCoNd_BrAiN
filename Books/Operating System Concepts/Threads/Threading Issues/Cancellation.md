---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.4.2"]
---

# Cancellation

1) **Thread cancellation** is the task of terminating a thread before it has completed. For example, if multiple threads are concurrently searching through a database and one thread returns the result, the remaining threads might be canceled. 
2) Another situation might occur when a user presses a button on a Web browser that stops a Web page from loading any further. Often, a Web page is loaded using several threads—each image is loaded in a separate thread. When a user presses the _stop_ button on the browser, all threads loading the page are canceled.
3) A thread that is to be canceled is often referred to as the **target thread**. Cancellation of a target thread may occur in two different scenarios:
	1. **Asynchronous cancellation**. One thread immediately terminates the target thread.
	2. **Deferred cancellation**. The target thread periodically checks whether it should terminate, allowing it an opportunity to terminate itself in an orderly fashion.
4) The difficulty with cancellation occurs in situations where resources have been allocated to a canceled thread or where a thread is canceled while in the midst of updating data it is sharing with other threads. This becomes especially troublesome with asynchronous cancellation. Often, the operating system will reclaim system resources from a canceled thread but will not reclaim all resources. Therefore, canceling a thread asynchronously may not free a necessary system-wide resource.
5) With deferred cancellation, in contrast, one thread indicates that a target thread is to be canceled, but cancellation occurs only after the target thread has checked a flag to determine whether or not it should be canceled. The thread can perform this check at a point at which it can be canceled safely. Pthreads refers to such points as **cancellation points**.

## Related

- [[> Threading Issues]] — back to the sub-topic MOC
- [[The fork() and exec() System Calls]] — the previous threading issue
- [[Signal Handling]] — the next threading issue
- [[Pthreads]] — the library that defines cancellation points
