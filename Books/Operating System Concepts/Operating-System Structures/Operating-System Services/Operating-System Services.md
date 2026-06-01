---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["Operating-System Services"]
---

# Operating-System Services
1) An operating system provides an environment for the execution of programs. It provides certain services to programs and to the users of those programs.
2) The specific services provided, of course, differ from one operating system to another, but we can identify common classes.
3) These operating-system services are provided for the convenience of the programmer, to make the programming task easier. 
4) The figure shows one view of the various operating-system services and how they interrelate.
![[Pasted image 20260527220041.png]]

5) One set of operating-system services provides functions that are helpful to the user.
	- **User interface**: 
		- Almost all operating systems have a **user interface (UI)**. 
		- This interface can take several forms. 
			- One is a **DTrace** command-line **interface (CLI)**, which uses text commands and a method for entering them.
			- Another is a **batch interface**, in which commands and directives to control those commands are entered into files, and those files are executed. 
			- Most commonly, a **graphical user interface (GUI)** is used.
	- **Program execution**: 
		- The system must be able to load a program into memory and to run that program.
		- The program must be able to end its execution, either normally or abnormally (indicating error).
	- **I/O operations**: 
		- A running program may require I/O, which may involve a file or an [[IO Systems|I/O device]].
		- For specific devices, special functions may be desired (such as recording to a CD or DVD drive or blanking a display screen). For efficiency and protection, users usually cannot control I/O devices directly. Therefore, the operating system must provide a means to do I/O.
	- **File-system manipulation**: 
		- Programs need to read and write files and directories. They also need to create and delete them by name, search for a given file, and list file information.
		- Finally, some programs include permissions management to allow or deny access to files or directories based on file ownership. 
		- Many operating systems provide a variety of [[File-System Management|file systems]], sometimes to allow personal choice, and sometimes to provide specific features or performance characteristics.
	- **Communications**:
		- There are many circumstances in which one [[process]] needs to exchange information with another process.
		- Such communication may occur between processes that are executing on the same computer or between processes that are executing on different computer systems tied together by a computer network.
		- Communications may be implemented via _[[shared memory]]_ or through _[[message passing]],_ in which packets of information are moved between processes by the operating system.
	- **Error detection**: 
		- The operating system needs to be constantly aware of possible errors.
		- Errors may occur in the CPU and memory hardware (such as a memory error or a power failure), in I/O devices (such as a parity error on tape, a connection failure on a network, or lack of paper in the printer), and in the user program (such as an arithmetic overflow, an attempt to access an illegal memory location, or a too-great use of CPU time). 
		- For each type of error, the operating system should take the appropriate action to ensure correct and consistent computing.
	
	Another set of operating-system functions exists for ensuring the efficient operation of the system itself. Systems with multiple users can gain efficiency by sharing the computer resources among the users.
	- **Resource allocation**: 
		- When there are multiple users or multiple jobs running at the same time, resources must be allocated to each of them. 
		- Many different types of resources are managed by the operating system. 
		- Some (such as CPU cycles, main memory, and file storage) may have special allocation code, whereas others (such as I/O devices) may have much more general request and release code.
	- **Accounting**: 
		- We want to keep track of which users use how much and what kinds of computer resources. 
		- This record keeping may be used for accounting (so that users can be billed) or simply for accumulating usage statistics. 	
	- **Protection and security**: 
		- The owners of information stored in a multi user or networked computer system may want to control use of that information.
		- When several separate processes execute concurrently, it should not be possible for one process to interfere with the others or with the operating system itself. 
		- [[Protection]] involves ensuring that all access to system resources is controlled. 
		- [[Security]] of the system from outsiders is also important. Such security starts with requiring each user to authenticate himself or herself to the system, usually by means of a password, to gain access to system resources.
		- It extends to defending external I/O devices, including modems and network adapters, from invalid access attempts and to recording all such connections for detection of break-ins. 
		- If a system is to be protected and secure, precautions must be instituted throughout it. A chain is only as strong as its weakest link.

## Related

- [[> Operating-System Services]] — back to the sub-topic MOC
- [[Protection]] — controlled access to system resources
- [[Security]] — defending the system from outsiders
- [[IO Systems]] — how the OS handles I/O operations and devices
- [[File-System Management]] — file-system services in more detail
