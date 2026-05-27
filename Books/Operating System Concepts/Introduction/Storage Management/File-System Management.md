---
tags: [book, os, operating-systems, book-os-concepts, file-systems]
---

# File-System Management

1) File management is one of the most visible components of an operating system. Computers can store information on several different types of physical media.  
2) Each of these media has its own characteristics and physical organization. 
3) A file is a collection of related information defined by its creator. Commonly, files represent programs (both source and object forms) and data.
4) The operating system implements the abstract concept of a file by managing [[Mass-Storage Management|mass-storage media]] and the devices that control them. Also, files are normally organized into directories to make them easier to use. Finally, when multiple users have access to files, it may be desirable to control by whom and in what ways (for example, read, write, append) files may be accessed.

The operating system is responsible for the following activities in connection with file management:
• Creating and deleting files
• Creating and deleting directories to organize files
• Supporting primitives for manipulating files and directories
• Mapping files onto secondary storage
• Backing up files on stable (nonvolatile) storage media

## Related

- [[> Storage Management]] — back to the sub-topic MOC
- [[Mass-Storage Management]] — secondary and tertiary storage management
- [[Caching]] — caching principles and cache management
