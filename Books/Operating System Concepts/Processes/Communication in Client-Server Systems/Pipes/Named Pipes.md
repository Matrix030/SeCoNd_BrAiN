---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.6.3.2", "Named Pipes", "FIFO"]
---

# Named Pipes

1) Ordinary pipes provide a simple communication mechanism between a pair of processes. However, ordinary pipes exist only while the processes are communicating with one another. On both UNIX and Windows systems, once the processes have finished communicating and terminated, the ordinary pipe ceases to exist.

2) Named pipes provide a much more powerful communication tool; communication can be bidirectional, and no parent-child relationship is required. Once a named pipe is established, several processes can use it for communication. In fact, in a typical scenario, a named pipe has several writers. Additionally, named pipes continue to exist after communicating processes have finished. Both UNIX and Windows systems support named pipes, although the details of implementation vary greatly. Next, we explore named pipes in each of these systems.

3) Named pipes are referred to as **FIFOs** in UNIX systems. Once created, they appear as typical files in the file system. A FIFO is created with the mkfifo() system call and manipulated with the ordinary open(), read(), write(), and close() system calls. It will continue to exist until it is explicitly deleted from the file system. Although FIFOs allow bidirectional communication, only half-duplex transmission is permitted. If data must travel in both directions, two FIFOs are typically used. Additionally, the communicating processes must reside on the same machine; sockets (Section 3.6.1) must be used if intermachine communication is required.
4) Named pipes on Windows systems provide a richer communication mechanism than their UNIX counterparts. Full-duplex communication is allowed, and the communicating processes may reside on either the same or different machines. Additionally, only byte-oriented data may be transmitted across a UNIX FIFO, whereas Windows systems allow either byte- or message-oriented data. Named pipes are created with the CreateNamedPipe() function, and a client can connect to a named pipe using ConnectNamedPipe(). Communication over the named pipe can be accomplished using the ReadFile() and WriteFile() functions.

> [!note] PIPES IN PRACTICE
> Pipes are used quite often in the UNIX command-line environment for situations in which the output of one command serves as input to the second. For example, the UNIX ls command produces a directory listing. For especially long directory listings, the output may scroll through several screens. The command more manages output by displaying only one screen of output at a time; the user must press the space bar to move from one screen to the next. Setting up a pipe between the ls and more commands (which are running as individual processes) allows the output of ls to be delivered as the input to more, enabling the user to display a large directory listing a screen at a time. A pipe can be constructed on the command line using the | character. The complete command is
>
> ```bash
> ls | more
> ```
>
> In this scenario, the ls command serves as the producer, and its output is consumed by the more command.
>
> Windows systems provide a more command for the DOS shell with functionality similar to that of its UNIX counterpart. The DOS shell also uses the | character for establishing a pipe. The only difference is that to get a directory listing, DOS uses the dir command rather than ls. The equivalent command in DOS to what is shown above is
>
> ```bash
> dir | more
> ```

## Related

- [[> Pipes]] — back to the section MOC
- [[Ordinary Pipes]] — the simpler, unidirectional counterpart
- [[Sockets]] — required instead of FIFOs for intermachine communication
