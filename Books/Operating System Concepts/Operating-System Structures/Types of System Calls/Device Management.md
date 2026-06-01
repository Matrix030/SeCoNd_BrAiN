---
tags: [book, os, operating-systems, book-os-concepts, io]
aliases: ["2.4.3"]
---

# Device Management

1) A [[process]] may need several resources to execute—main memory, disk drives, access to files, and so on. If the resources are available, they can be granted, and control can be returned to the user process. Otherwise, the process will have to wait until sufficient resources are available.
2) The various resources controlled by the operating system can be thought of as devices. Some of these devices are physical devices (for example, disk drives), while others can be thought of as abstract or virtual devices (for example, files).
3) A system with multiple users may require us to first request the device, to ensure exclusive use of it. After we are finished with the device, we release it. These functions are similar to the open and close system calls for files. 
4) Once the device has been requested (and allocated to us), we can read, write, and (possibly) reposition the device, just as we can with files. In fact, the similarity between [[IO Systems|I/O devices]] and files is so great that many operating systems, including UNIX, merge the two into a combined file-device structure. In this case, a set of system calls is used on both files and devices. Sometimes, I/O devices are identified by special file names, directory placement, or file attributes.
5) The user interface can also make files and devices appear to be similar, even though the underlying system calls are dissimilar. This is another example of the many design decisions that go into building an operating system and user interface.

## Related

- [[> Types of System Calls]] — back to the sub-topic MOC
- [[File Management]] — the previous category of system calls
- [[Information Maintenance]] — the next category of system calls
- [[IO Systems]] — how the OS handles I/O devices
