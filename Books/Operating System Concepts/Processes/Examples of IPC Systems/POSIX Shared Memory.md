---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["3.5.1", "An Example: POSIX Shared Memory"]
---

# POSIX Shared Memory

1) Several IPC mechanisms are available for POSIX systems, including shared memory and message passing. Here, we explore the POSIX API for shared memory.
2) A process must first create a shared memory segment using the **shmget()** system call (shmget() is derived from SHared Memory GET). The following example illustrates the use of shmget():

```c
segment_id = shmget(IPC_PRIVATE, size, S_IRUSR | S_IWUSR);
```

3) This first parameter specifies the **key** (or identifier) of the shared-memory segment. If this is set to IPC_PRIVATE, a new shared-memory segment is created. The second parameter specifies the **size** (in bytes) of the shared-memory segment. Finally, the third parameter identifies the **mode**, which indicates how the shared-memory segment is to be used—that is, for reading, writing, or both. By setting the mode to S_IRUSR | S_IWUSR, we are indicating that the owner may read or write to the shared-memory segment. A successful call to shmget() returns an integer identifier for the shared-memory segment. Other processes that want to use this region of shared memory must specify this identifier.
4) Processes that wish to access a shared-memory segment must attach it to their address space using the **shmat()** (SHared Memory ATtach) system call. The call to shmat() expects three parameters as well. The first is the integer identifier of the shared-memory segment being attached, and the second is a pointer location in memory indicating where the shared memory will be attached. If we pass a value of NULL, the operating system selects the location on the user’s behalf. The third parameter identifies a flag that allows the shared-memory region to be attached in read-only or read-write mode; by passing a parameter of 0, we allow both reads and writes to the shared region. We attach a region of shared memory using shmat() as follows:

```c
shared_memory = (char *) shmat(id, NULL, 0);
```

5) If successful, shmat() returns a pointer to the beginning location in memory where the shared-memory region has been attached.
6) Once the region of shared memory is attached to a process’s address space, the process can access the shared memory as a routine memory access using the pointer returned from shmat(). In this example, shmat() returns a pointer to a character string. Thus, we could write to the shared-memory region as follows:

```c
sprintf(shared_memory, "Writing to shared memory");
```

7) Other processes sharing this segment would see the updates to the shared-memory segment.
8) Typically, a process using an existing shared-memory segment first attaches the shared-memory region to its address space and then accesses (and possibly updates) the region of shared memory. When a process no longer requires access to the shared-memory segment, it detaches the segment from its address space. To detach a region of shared memory, the process can pass the pointer of the shared-memory region to the **shmdt()** system call, as follows:

```c
shmdt(shared_memory);
```

9) Finally, a shared-memory segment can be removed from the system with the **shmctl()** system call, which is passed the identifier of the shared segment along with the flag IPC_RMID.
10) The program shown below illustrates the POSIX shared-memory API just discussed. This program creates a 4,096-byte shared-memory segment. Once the region of shared memory is attached, the process writes the message Hi There! to shared memory. After outputting the contents of the updated memory, it detaches and removes the shared-memory region. We provide further exercises using the POSIX shared-memory API in the programming exercises at the end of this chapter.

```c
#include <stdio.h>
#include <sys/shm.h>
#include <sys/stat.h>

int main()
{
/* the identifier for the shared memory segment */
int segment_id;
/* a pointer to the shared memory segment */
char *shared_memory;
/* the size (in bytes) of the shared memory segment */
const int size = 4096;

	/* allocate a shared memory segment */
	segment_id = shmget(IPC_PRIVATE, size, S_IRUSR | S_IWUSR);

	/* attach the shared memory segment */
	shared_memory = (char *) shmat(segment_id, NULL, 0);

	/* write a message to the shared memory segment */
	sprintf(shared_memory, "Hi there!");

	/* now print out the string from shared memory */
	printf("*%s\n", shared_memory);

	/* now detach the shared memory segment */
	shmdt(shared_memory);

	/* now remove the shared memory segment */
	shmctl(segment_id, IPC_RMID, NULL);

	return 0;
}
```

## Related

- [[> Examples of IPC Systems]] — back to the sub-topic MOC
- [[Shared-Memory Systems]] — the shared-memory IPC model this API implements
- [[Mach]] — the message-passing example that follows
