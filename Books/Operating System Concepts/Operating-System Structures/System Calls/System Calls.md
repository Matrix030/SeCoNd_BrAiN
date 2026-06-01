---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["System Call"]
---

# System Calls

1) **System calls** provide an interface to the services made available by an [[Operating-System Services|operating system]]. These calls are generally available as routines written in C and C++, although certain low-level tasks (for example, tasks where hardware must be accessed directly), may need to be written using assembly-language instructions.
2) Before we discuss how an operating system makes system calls available, let's first use an example to illustrate how system calls are used: 
	- writing a simple program to read data from one file and copy them to another file. The first input that the program will need is the names of the two files: the input file and the output file. 
	- These names can be specified in many ways, depending on the operating-system design. 
		- One approach is for the program to ask the user for the names of the two files. 
		- In an interactive system, this approach will require a sequence of system calls, first to write a prompting message on the screen and then to read from the keyboard the characters that define the two files.
		- On mouse-based and icon-based systems, a menu of file names is usually displayed in a window. The user can then use the mouse to select the source name, and a window can be opened for the destination name to be specified. This sequence requires many [[IO Systems|I/O]] system calls.
	- Once the two file names are obtained, the program must open the input file and create the output file.
	- Each of these operations requires another system call. There are also possible error conditions for each operation. 
	- When the program tries to open the input file, it may find that there is no file of that name or that the file is protected against access. 
	- In these cases, the program should print a message on the console (another sequence of system calls) and then terminate abnormally (another system call). 
	- If the input file exists, then we must create a new output file. We may find that there is already an output file with the same name. This situation may cause the program to abort (a system call), or we may delete the existing file (another system call) and create a new one (another system call).
	- Another option, in an interactive system, is to ask the user (via a sequence of system calls to output the prompting message and to read the response from the terminal) whether to replace the existing file or to abort the program.
3) Now that both files are set up, we enter a loop that reads from the input file (a system call) and writes to the output file (another system call). 
4) Each read and write must return status information regarding various possible error conditions. On input, the program may find that the end of the file has been reached or that there was a hardware failure in the read (such as a parity error). The write operation may encounter various errors, depending on the output device (no more disk space, printer out of paper, and so on).
5) Finally, after the entire file is copied, the program may close both files (another system call), write a message to the console or window (more system calls), and finally terminate normally (the final system call). 
6) As we can see, even simple programs may make heavy use of the operating system. Frequently, systems execute thousands of system calls per second. This system-call sequence is shown in the Figure below.

![[Pasted image 20260528141057.png]]

7) Behind the scenes, the functions that make up an [[API]] typically invoke the actual system calls on behalf of the application programmer. 
8) Why would an application programmer prefer programming according to an API rather than invoking actual system calls? 
	- One benefit of programming according to an API concerns program portability: An application programmer designing a program using an API can expect her program to compile and run on any system that supports the same API.
	- Furthermore, actual system calls can often be more detailed and difficult to work with than the API available to an application programmer.

9) The run-time support system (a set of functions built into libraries included with a compiler) for most programming languages provides a **system-call interface** that serves as the link to system calls made available by the operating system.
10) The system-call interface intercepts function calls in the API and invokes the necessary system calls within the operating system. 
11) Typically, a number is associated with each system call, and the system-call interface maintains a table indexed according to these numbers. 
12) The system call interface then invokes the intended system call in the operating-system [[kernel]] and returns the status of the system call and any return values.
13) The caller need know nothing about how the system call is implemented or what it does during execution. Rather, it need only obey the API and understand what the operating system will do as a result of the execution of that system call. 
14) most of the details of the operating-system interface are hidden from the programmer by the API and are managed by the run-time support library. The relationship between an API, the system-call interface, and the operating system is shown in the Figure below, which illustrates how the operating system handles a user application invoking the open() system call.

![[Pasted image 20260528141132.png]]

15) System calls occur in different ways, depending on the computer in use. Often, more information is required than simply the identity of the desired system call. The exact type and amount of information vary according to the particular operating system and call. For example, to get input, we may need to specify the file or device to use as the source, as well as the address and length of the memory buffer into which the input should be read. Of course, the device or file and length may be implicit in the call.
16) Three general methods are used to pass parameters to the operating system. 
	- The simplest approach is to pass the parameters in _registers_. In some cases, however, there may be more parameters than registers. In these cases, the parameters are generally stored in a _block,_ or table, in memory, and the address of the block is passed as a parameter in a register (refer the figure below). 
	- Parameters also can be placed, or _pushed,_ onto the _stack_ by the program and _popped_ off the stack by the operating system. 
	- Some operating systems prefer the block or stack method because those approaches do not limit the number or length of parameters being passed.
	- ![[Pasted image 20260528145351.png]]

## Related

- [[> System Calls]] — back to the sub-topic MOC
- [[Operating-System Services]] — the services that system calls expose
- [[Command Interpreter]] — uses system calls to execute user commands
- [[Dual-Mode Operation]] — how system calls cross from user mode to kernel mode
