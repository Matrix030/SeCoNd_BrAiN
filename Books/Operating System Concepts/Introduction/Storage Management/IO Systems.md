---
tags: [book, os, operating-systems, book-os-concepts, io]
aliases: ["I/O Systems"]
---

# I/O Systems

1) One of the purposes of an operating system is to hide the peculiarities of specific hardware devices from the user. For example, in UNIX, the peculiarities of I/O devices are hidden from the bulk of the operating system itself by the **I/O subsystem**. The I/O subsystem consists of several components:
	• A memory-management component that includes buffering, [[Caching|caching]], and spooling
	• A general device-driver interface
	• Drivers for specific hardware devices

2) Only the device driver knows the peculiarities of the specific device to which it is assigned.

## Related

- [[> Storage Management]] — back to the sub-topic MOC
- [[Caching]] — caching as a component of the I/O subsystem
- [[Mass-Storage Management]] — storage that I/O systems manage access to
