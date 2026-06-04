---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.5.3", "An Example: Windows XP"]
---

# Windows XP

1) The **Windows XP** operating system is an example of modern design that employs modularity to increase functionality and decrease the time needed to implement new features. Windows XP provides support for multiple operating environments, or _subsystems,_ with which application programs communicate via a message-passing mechanism. The application programs can be considered clients of the Windows XP subsystem server.
2) The message-passing facility in Windows XP is called the **local procedure-call (LPC)** facility. The LPC in Windows XP communicates between two processes on the same machine. It is similar to the standard RPC mechanism that is widely used, but it is optimized for and specific to Windows XP. Like Mach, Windows XP uses a **port object** to establish and maintain a connection between two processes. Every client that calls a subsystem needs a communication channel, which is provided by a port object and is never inherited. Windows XP uses two types of ports: **connection ports** and **communication ports**. They are really the same but are given different names according to how they are used.
3) Connection ports are named _objects_ and are visible to all processes; they give applications a way to set up communication channels (Chapter 22). The communication works as follows:
	- The client opens a handle to the subsystem’s connection port object.
	- The client sends a connection request.
	- The server creates two private communication ports and returns the handle to one of them to the client.
	- The client and server use the corresponding port handle to send messages or callbacks and to listen for replies.
4) Windows XP uses two types of message-passing techniques over a port that the client specifies when it establishes the channel. The simplest, which is used for small messages, uses the port’s message queue as intermediate storage and copies the message from one process to the other. Under this method, messages of up to 256 bytes can be sent.
5) If a client needs to send a larger message, it passes the message through a **section object**, which sets up a region of shared memory. The client has to decide when it sets up the channel whether or not it will need to send a large message. If the client determines that it does want to send large messages, it asks for a section object to be created. Similarly, if the server decides that replies will be large, it creates a section object. So that the section object can be used, a small message is sent that contains a pointer and size information about the section object. This method is more complicated than the first method, but it avoids data copying. In both cases, a **callback mechanism** can be used when either the client or the server cannot respond immediately to a request. The callback mechanism allows them to perform asynchronous message handling. The structure of local procedure calls in Windows XP is shown in Figure 3.17.
6) It is important to note that the LPC facility in Windows XP is not part of the Win32 API and hence is not visible to the application programmer. Rather, applications using the Win32 API invoke standard remote procedure calls. When the RPC is being invoked on a process on the same system, the RPC is indirectly handled through a local procedure call. LPCs are also used in a few other functions that are part of the Win32 API.

![[Pasted image 20260604185000.png]]

## Related

- [[> Examples of IPC Systems]] — back to the sub-topic MOC
- [[Mach]] — the message-passing example that also uses ports
- [[Shared-Memory Systems]] — the model behind section objects for large messages
