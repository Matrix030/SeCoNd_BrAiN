---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.4.4"]
---

# Thread Pools

1) In Section 4.1, we mentioned multithreading in a Web server. In this situation, whenever the server receives a request, it creates a separate thread to service the request. Whereas creating a separate thread is certainly superior to creating a separate process, a multithreaded server nonetheless has potential problems. 
2) The first issue concerns the amount of time required to create the thread prior to servicing the request, together with the fact that this thread will be discarded once it has completed its work. The second issue is more troublesome: if we allow all concurrent requests to be serviced in a new thread, we have not placed a bound on the number of threads concurrently active in the system. 
3) Unlimited threads could exhaust system resources, such as CPU time or memory. One solution to this problem is to use a **thread pool**.
4) The general idea behind a thread pool is to create a number of threads at process startup and place them into a _pool,_ where they sit and wait for work. 
5) When a server receives a request, it awakens a thread from this pool—if one is available—and passes it the request for service. Once the thread completes its service, it returns to the pool and awaits more work. If the pool contains no available thread, the server waits until one becomes free.

Thread pools offer these benefits:
	1. Servicing a request with an existing thread is usually faster than waiting to create a thread.
	2. A thread pool limits the number of threads that exist at any one point. This is particularly important on systems that cannot support a large number of concurrent threads.
1) The number of threads in the pool can be set heuristically based on factors such as the number of CPUs in the system, the amount of physical memory, and the expected number of concurrent client requests. 
2) More sophisticated thread-pool architectures can dynamically adjust the number of threads in the pool according to usage patterns. Such architectures provide the further benefit of having a smaller pool—thereby consuming less memory—when the load on the system is low.
3) The Win32 API provides several functions related to thread pools. Using the thread pool API is similar to creating a thread with the Thread Create() function, as described in Section 4.3.2. Here, a function that is to run as a separate thread is defined. Such a function may appear as follows:

```c
DWORD WINAPI PoolFunction(PVOID Param) {
	/**
	 * this function runs as a separate thread.
	 **/
}
```

4) A pointer to PoolFunction() is passed to one of the functions in the thread pool API, and a thread from the pool executes this function. One such member in the thread pool API is the QueueUserWorkItem() function, which is passed three parameters:
	• LPTHREAD_START_ROUTINE Function—a pointer to the function that is to run as a separate thread
	• PVOID Param—the parameter passed to Function
	• ULONG Flags—flags indicating how the thread pool is to create and manage execution of the thread

An example of invoking a function is:

```c
QueueUserWorkItem(&PoolFunction, NULL, 0);
```

5) This causes a thread from the thread pool to invoke PoolFunction() on behalf of the programmer. In this instance, we pass no parameters to PoolFunction (). Because we specify 0 as a flag, we provide the thread pool with no special instructions for thread creation.
6) Other members in the Win32 thread pool API include utilities that invoke functions at periodic intervals or when an asynchronous I/O request completes. The java.util.concurrent package in Java 1.5 provides a thread pool utility as well.

## Related

- [[> Threading Issues]] — back to the sub-topic MOC
- [[Signal Handling]] — the previous threading issue
- [[Thread-Specific Data]] — the next threading issue
- [[Motivation]] — the multithreaded Web server from Section 4.1
- [[Win32 Threads]] — the thread-creation API this builds on
