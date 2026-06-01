---
tags: [book, os, operating-systems, book-os-concepts, file-systems]
aliases: ["2.4.2"]
---

# File Management

1) We first need to be able to create and delete files. Either system call requires the name of the file and perhaps some of the file’s attributes. Once the file is created, we need to open it and to use it. We may also read, write, or reposition (rewinding or skipping to the end of the file, for example). Finally, we need to close the file, indicating that we are no longer using it.
2) We may need these same sets of operations for directories if we have a directory structure for organizing files in the [[File-System Management|file system]]. In addition, for either files or directories, we need to be able to determine the values of various attributes and perhaps to reset them if necessary. File attributes include the file name, file type, protection codes, accounting information, and so on. 
3) At least two system calls, get file attribute and set file attribute, are required for this function. Some operating systems provide many more calls, such as calls for file move and copy. Others might provide an [[API]] that performs those operations using code and other system calls, and others might just provide system programs to perform those tasks. If the system programs are callable by other programs, then each can be considered an API by other system programs.

## Related

- [[> Types of System Calls]] — back to the sub-topic MOC
- [[Process Control]] — the previous category of system calls
- [[Device Management]] — the next category of system calls
- [[File-System Management]] — file-system services in more detail
