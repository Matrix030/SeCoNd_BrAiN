---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.7.3"]
---

# Microkernels

1) We have already seen that as UNIX expanded, the kernel became large and difficult to manage.
2) In the mid-1980s, researchers at Carnegie Mellon University developed an operating system called **Mach** that modularized the kernel using the **microkernel** approach. 
3) This method structures the operating system by removing all nonessential components from the kernel and implementing them as system and user-level programs.
4) The result is a smaller kernel. There is little consensus regarding which services should remain in the kernel and which should be implemented in user space. Typically, however, microkernels provide minimal process and memory management, in addition to a communication facility.
5) The **main function of the microkernel** is to provide a communication facility between the client program and the various services that are also running in user space. 
6) Communication is provided by _message passing,_ which was described in Section 2.4.5. For example, if the client program wishes to access a file, it must interact with the file server. The client program and service never interact directly. Rather, they communicate indirectly by exchanging messages with the microkernel.
7) One benefit of the microkernel approach is ease of extending the operating system. All new services are added to user space and consequently do not require modification of the kernel. When the kernel does have to be modified, the changes tend to be fewer, because the microkernel is a smaller kernel.
8) The resulting operating system is easier to port from one hardware design to another. The microkernel also provides more security and reliability, since most services are running as user—rather than kernel—processes. If a service fails, the rest of the operating system remains untouched.
9) Unfortunately, microkernels can suffer from performance decreases due to increased system function overhead. Consider the history of Windows NT. 

## Related

- [[> Operating-System Structure]] — back to the sub-topic MOC
- [[Layered Approach]] — another modular strategy
- [[Modules]] — combines microkernel ideas without message-passing overhead
