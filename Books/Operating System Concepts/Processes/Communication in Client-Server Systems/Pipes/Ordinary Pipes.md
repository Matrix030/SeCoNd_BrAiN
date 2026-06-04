---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.6.3.1", "Ordinary Pipes", "Anonymous Pipes"]
---

# Ordinary Pipes

1) Ordinary pipes allow two processes to communicate in standard producer-consumer fashion; the producer writes to one end of the pipe (the **write-end**), and the consumer reads from the other end (the **read-end**). As a result, ordinary pipes are unidirectional, allowing only one-way communication. If two-way communication is required, two pipes must be used, with each pipe sending data in a different direction. We next illustrate constructing ordinary pipes on both UNIX and Windows systems. In both program examples, one process writes the message Greetings to the pipe, while the other process reads this message from the pipe.
2) On UNIX systems, ordinary pipes are constructed using the function

![[Pasted image 20260604185733.png]]

3) This function creates a pipe that is accessed through the int fd[] file descriptors: fd[0] is the read-end of the pipe, and fd[1] is the write end. UNIX treats a pipe as a special type of file; thus, pipes can be accessed using ordinary read() and write() system calls.
4) An ordinary pipe cannot be accessed from outside the process that creates it. Thus, typically a parent process creates a pipe and uses it to communicate with a child process it creates via fork(). Recall from Section 3.3.1 that a child process inherits open files from its parent. Since a pipe is a special type of file, the child inherits the pipe from its parent process. Figure 3.22 illustrates the relationship of the file descriptor fd to the parent and child processes.

![[Pasted image 20260604185741.png]]

5) In the UNIX program shown in Figure 3.23, the parent process creates a pipe and then sends a fork() call creating the child process. What occurs after the fork() call depends on how the data are to flow through the pipe. In this instance, the parent writes to the pipe and the child reads from it. It is important to notice that both the parent process and the child process initially close their unused ends of the pipe. Although the program shown in Figure 3.23 does not require this action, it is an important step to ensure that a process reading from the pipe can detect end-of-file (read() returns 0) when the writer has closed its end of the pipe.

![[Pasted image 20260604185751.png]]

![[Pasted image 20260604185758.png]]

6) Ordinary pipes on Windows systems are termed **anonymous pipes**, and they behave similarly to their UNIX counterparts: they are unidirectional and employ parent-child relationships between the communicating processes. In addition, reading and writing to the pipe can be accomplished with the ordinary ReadFile() and WriteFile() functions. The Win32 API for creating pipes is the CreatePipe() function, which is passed four parameters: separate handles for (1) reading and (2) writing to the pipe, as well as (3) an instance of the STARTUPINFO structure, which is used to specify that the child process is to inherit the handles of the pipe. Furthermore, (4) the size of the pipe (in bytes) may be specified.
7) Figure 3.25 illustrates a parent process creating an anonymous pipe for communicating with its child. Unlike UNIX systems, in which a child process automatically inherits a pipe created by its parent, Windows requires the programmer to specify which attributes the child process will inherit. This is accomplished by first initializing the SECURITY_ATTRIBUTES structure to allow handles to be inherited and then redirecting the child process’s handles for standard input or standard output to the read or write handle of the pipe. Since the child will be reading from the pipe, the parent must redirect the child’s standard input to the read handle of the pipe. Furthermore, as the pipes are half duplex, it is necessary to prohibit the child from inheriting the write end of the pipe. Creating the child process is similar to the program in Figure 3.12, except that the fifth parameter is set to TRUE, indicating that the child process is to inherit designated handles from its parent. Before writing to the pipe, the parent first closes its unused read end of the pipe. The child process that reads from the pipe is shown in Figure 3.27. Before reading from the pipe, this program obtains the read handle to the pipe by invoking GetStdHandle().

![[Pasted image 20260604185812.png]]

8) Note that ordinary pipes require a parent-child relationship between the communicating processes on both UNIX and Windows systems. This means that these pipes can be used only for communication between processes on the same machine.

## Related

- [[> Pipes]] — back to the section MOC
- [[Named Pipes]] — the bidirectional, persistent counterpart
- [[Operations on Processes]] — fork() and how a child inherits open files
