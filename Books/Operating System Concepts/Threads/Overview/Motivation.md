---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.1.1"]
---

# Motivation

1) Many software packages that run on modern desktop PCs are multithreaded. An application typically is implemented as a separate process with several threads of control. 
	- A Web browser might have one thread display images or text 
	- while another thread retrieves data from the network, for example.
2) A word processor may have a thread for displaying graphics, another thread for responding to keystrokes from the user, and a third thread for performing spelling and grammar checking in the background.

![[Pasted image 20260607095425.png]]

3) In certain situations, a single application may be required to perform several similar tasks. 
4) For example, a Web server accepts client requests for Web pages, images, sound, and so forth. 
5) A busy Web server may have several (perhaps thousands of) clients concurrently accessing it. If the Web server ran as a traditional single-threaded process, it would be able to service only one client at a time, and a client might have to wait a very long time for its request to be serviced.
6) One solution is to have the server run as a single process that accepts requests. When the server receives a request, it creates a separate process to service that request.
7) In fact, this process-creation method was in common use before threads became popular. Process creation is time consuming and resource intensive, however. If the new process will perform the same tasks as the existing process, why incur all that overhead? 
8) It is generally more efficient to use one process that contains multiple threads. If the Web-server process is multithreaded, the server will create a separate thread that listens for client requests. 
9) When a request is made, rather than creating another process, the server will create a new thread to service the request and resume listening for additional requests. This is illustrated in the figure below.
10) Threads also play a vital role in remote procedure call (RPC) systems. RPCs allow interprocess communication by providing a communication mechanism similar to ordinary function or procedure calls. Typically, RPC servers are multithreaded. When a server receives a message, it services the message using a separate thread. This allows the server to service several concurrent requests.
11) Finally, most operating system kernels are now multithreaded; several threads operate in the kernel, and each thread performs a specific task, such as managing devices or interrupt handling.

![[Pasted image 20260607095439.png]]

## Related

- [[> Overview]] — back to the sub-topic MOC
- [[Single-Threaded and Multithreaded Processes]] — the section preamble
- [[Benefits]] — the payoff of multithreading
- [[Remote Procedure Calls]] — the RPC servers mentioned here
- [[Process Creation]] — the costlier alternative threads replace
