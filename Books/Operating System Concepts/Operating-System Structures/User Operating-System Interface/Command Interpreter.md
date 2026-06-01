---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["Shell", "2.2.1"]
---

# Command Interpreter

1) On systems with multiple command interpreters to choose from, the interpreters are known as **shells**. For example, on UNIX and Linux systems, a user may choose among several different shells, including the _Bourne shell_, _C shell_, _Bourne-Again shell. 
2) Most shells provide similar functionality, and a user's choice of which shell to use is generally based on personal preference.Figure shows the Bourne shell command interpreter being used.
3) The main function of the command interpreter is to get and execute the next user-specified command. 
4) Many of the commands given at this level manipulate files: create, delete, list, print, copy, execute, and so on.
5) In one approach, the command interpreter itself contains the code to execute the command. For example, a command to delete a file may cause the command interpreter to jump to a section of its code that sets up the parameters and makes the appropriate [[system call]]. In this case, the number of commands that can be given determines the size of the command interpreter, since each command requires its own implementing code.
6) An alternative approach—used by UNIX, among other operating systems —implements most commands through system programs. In this case, the command interpreter does not understand the command in any way; it merely uses the command to identify a file to be loaded into memory and executed. Thus, the UNIX command to delete a file
![[Pasted image 20260528140157.png|837]]
would search for a file called rm, load the file into memory, and execute it with the parameter file.txt. The function associated with the rm command would be defined completely by the code in the file rm.
7) In this way, programmers can add new commands to the system easily by creating new files with the proper names. The command-interpreter program, which can be small, does not have to be changed for new commands to be added.

![[Pasted image 20260528135242.png]]

## Related

- [[> User Operating-System Interface]] — back to the sub-topic MOC
- [[User Operating-System Interface]] — the section intro
- [[Graphical User Interfaces]] — the alternative interface approach
- [[system call]] — how command interpreters invoke OS services
