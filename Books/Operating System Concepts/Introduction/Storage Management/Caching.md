---
tags: [book, os, operating-systems, book-os-concepts, memory]
---

# Caching

1) **Caching** is an important principle of computer systems. Information is normally kept in some storage system (such as main memory). As it is used, it is copied into a faster storage system—the cache—on a temporary basis.
2) When we need a particular piece of information, we first check whether it is in the cache. If it is, we use the information directly from the cache; if it is not, we use the information from the source, putting a copy in the cache under the assumption that we will need it again soon.
3) In addition, internal programmable registers, such as index registers, provide a high-speed cache for main memory. The programmer (or compiler) implements the register-allocation and register-replacement algorithms to decide which information to keep in registers and which to keep in main memory.
4) There are also caches that are implemented totally in hardware. For instance, most systems have an instruction cache to hold the instructions expected to be executed next.
5) Without this cache, the CPU would have to wait several cycles while an instruction was fetched from main memory. 
6) For similar reasons, most systems have one or more high-speed data caches in the memory hierarchy. We are not concerned with these hardware-only caches in this text, since they are outside the control of the operating system.
7) Because caches have limited size, **cache management** is an important design problem. Careful selection of the cache size and of a replacement policy can result in greatly increased performance. Figure below compares storage performance in large workstations and small servers. 
![[Pasted image 20260526222851.png]]

8) Main memory can be viewed as a fast cache for secondary storage, since data in secondary storage must be copied into main memory for use, and data must be in main memory before being moved to secondary storage for safekeeping. 
9) The file-system data, which resides permanently on secondary storage, may appear on several levels in the storage hierarchy. At the highest level, the operating system may maintain a cache of file-system data in main memory.
10) In addition, electronic RAM disks (also known as **solid-state disks**) may be used for high-speed storage that is accessed through the file-system interface. The bulk of secondary storage is on magnetic disks. 
11) The magnetic-disk storage, in turn, is often backed up onto magnetic tapes or removable disks to protect against data loss in case of a hard-disk failure. 
12) Some systems automatically archive old file data from secondary storage to tertiary storage, such as tape jukeboxes, to lower the storage cost. 
13) The movement of information between levels of a storage hierarchy may be either **explicit or implicit**, depending on the hardware design and the controlling operating-system software. 
14) For instance, data transfer from cache to CPU and registers is usually a hardware function, with no operating-system intervention. In contrast, transfer of data from disk to memory is usually controlled by the operating system.
15) In a hierarchical storage structure, the same data may appear in different levels of the storage system. 
16) Suppose integer A is stored on disk and needs to be incremented by 1. The system first loads the disk block containing A into main memory, then into cache, and finally into a CPU register. At this point, copies of A exist on the disk, in memory, cache, and the register. After the CPU increments A in the register, the values in these locations temporarily differ until the updated value is written back to disk.
![[Pasted image 20260526223937.png]]

17) In a computing environment where only one process executes at a time, this arrangement poses no difficulties, since an access to integer A will always be to the copy at the highest level of the hierarchy. 
18) However, in a multitasking environment, where the CPU is switched back and forth among various processes, extreme care must be taken to ensure that, if several processes wish to access A, then each of these processes will obtain the most recently updated value of A.
19) The situation becomes more complicated in a multiprocessor environment where, in addition to maintaining internal registers, each of the CPUs also contains a local cache ([Figure 1.6](https://learning.oreilly.com/library/view/operating-system-concepts/9780470128725/silb_9780470128725_oeb_c01_r1.html#FIG-1.6-section-1-5-1-3-2)). In such an environment, a copy of A may exist simultaneously in several caches. Since the various CPUs can all execute concurrently, we must make sure that an update to the value of A in one cache is immediately reflected in all other caches where A resides.
20) This situation is called **cache coherency**, and it is usually a hardware problem (handled below the operating-system level).
21) In a distributed environment, the situation becomes even more complex. In this environment, several copies (or replicas) of the same file can be kept on different computers that are distributed in space. 
22) Since the various replicas may be accessed and updated concurrently, some distributed systems ensure that, when a replica is updated in one place, all other replicas are brought up to date as soon as possible.

## Related

- [[> Storage Management]] — back to the sub-topic MOC
- [[Mass-Storage Management]] — the storage hierarchy caching sits within
- [[IO Systems]] — I/O subsystem that works alongside caching
