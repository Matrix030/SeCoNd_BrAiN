---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["System Utilities", "2.5"]
---

# System Programs

1) Another aspect of a modern system is the collection of system programs. See figure below, which depicted the logical computer hierarchy.
![[Pasted image 20260529150559.png]]
2) **System programs**, also known as **system utilities**, provide a convenient environment for program development and execution. Some of them are simply user interfaces to [[system call|system calls]]; others are considerably more complex. They can be divided into these categories:
	- **[[File Management|File management]]**. These programs create, delete, copy, rename, print, dump, list, and generally manipulate files and directories.
	- **Status information**. Some programs simply ask the system for the date, time, amount of available memory or disk space, number of users, or similar status information. Others are more complex, providing detailed performance, logging, and debugging information. Typically, these programs format and print the output to the terminal or other output devices or files or display it in a window of the GUI. Some systems also support a **registry**, which is used to store and retrieve configuration information.
	- **File modification**. Several text editors may be available to create and modify the content of files stored on disk or other storage devices. There may also be special commands to search contents of files or perform transformations of the text.
	- **Programming-language support**. Compilers, assemblers, debuggers, and interpreters for common programming languages (such as C, C++, Java, Visual Basic, and PERL) are often provided to the user with the operating system.
	- **Program loading and execution**. Once a program is assembled or compiled, it must be loaded into memory to be executed. The system may provide absolute loaders, relocatable loaders, linkage editors, and overlay loaders. Debugging systems for either higher-level languages or machine language are needed as well.
- **[[Communication|Communications]]**. These programs provide the mechanism for creating virtual connections among processes, users, and computer systems. They allow users to send messages to one another’s screens, to browse Web pages, to send electronic-mail messages, to log in remotely, or to transfer files from one machine to another.
- In addition to systems programs, most operating systems are supplied with programs that are useful in solving common problems or performing common operations. Such **application programs** include Web browsers, word processors and text formatters, spreadsheets, database systems, compilers, plotting and statistical-analysis packages, and games.
- The view of the operating system seen by most users is defined by the application and system programs, rather than by the actual system calls.

## Related

- [[> System Programs]] — back to the sub-topic MOC
- [[> Types of System Calls]] — the system-call categories system programs wrap
- [[Command Interpreter]] — itself a system program that runs other programs
- [[Operating-System Services]] — the underlying services these programs expose
