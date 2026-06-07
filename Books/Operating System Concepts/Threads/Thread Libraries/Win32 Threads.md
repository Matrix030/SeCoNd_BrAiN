---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.3.2"]
---

# Win32 Threads

1) The technique for creating threads using the Win32 thread library is similar to the Pthreads technique in several ways. We illustrate the Win32 thread API in the C program shown below. Notice that we must include the windows.h header file when using the Win32 API.
2) Just as in the [[Pthreads]] version, data shared by the separate threads—in this case, Sum—are declared globally (the DWORD data type is an unsigned 32-bit integer). 
3) We also define the Summation() function that is to be performed in a separate thread. This function is passed a pointer to a void, which Win32 defines as LPVOID. The thread performing this function sets the global data Sum to the value of the summation from 0 to the parameter passed to Summation().
4) Threads are created in the Win32 API using the CreateThread() function, and—just as in Pthreads—a set of attributes for the thread is passed to this function. These attributes include security information, the size of the stack, and a flag that can be set to indicate if the thread is to start in a suspended state. 
5) In this program, we use the default values for these attributes (which do not initially set the thread to a suspended state and instead make it eligible to be run by the CPU scheduler). Once the summation thread is created, the parent must wait for it to complete before outputting the value of Sum, as the value is set by the summation thread. Recall that the [[Pthreads|Pthread program]] had the parent thread wait for the summation thread using the pthread_join() statement. We perform the equivalent of this in the Win32 API using the WaitForSingleObject() function, which causes the creating thread to block until the summation thread has exited.

```c
#include <windows.h>
#include <stdio.h>
DWORD Sum; /* data is shared by the thread(s) */
/* the thread runs in this separate function */
DWORD WINAPI Summation(LPVOID Param)
{
	DWORD Upper = *(DWORD*)Param;
	for (DWORD i = 0; i <= Upper; i++)
		Sum += i;
	return 0;
}

int main(int argc, char *argv[])
{
	DWORD ThreadId;
	HANDLE ThreadHandle;
	int Param;
	/* perform some basic error checking */
	if (argc != 2) {
		fprintf(stderr,"An integer parameter is required\n");
		return -1;
	}
	Param = atoi(argv[1]);
	if (Param < 0) {
		fprintf(stderr,"An integer >= 0 is required\n");
		return -1;
	}

	// create the thread
	ThreadHandle = CreateThread(
		NULL, // default security attributes
		0, // default stack size
		Summation, // thread function
		&Param, // parameter to thread function
		0, // default creation flags
		&ThreadId); // returns the thread identifier

	if (ThreadHandle != NULL) {
		// now wait for the thread to finish
		WaitForSingleObject(ThreadHandle,INFINITE);

		// close the thread handle
		CloseHandle(ThreadHandle);

		printf("sum = %d\n",Sum);
	}
}
```

## Related

- [[> Thread Libraries]] — back to the sub-topic MOC
- [[Pthreads]] — the POSIX equivalent this is compared against
- [[Java Threads]] — the equivalent Java API
- [[Thread Library Implementation]] — the section preamble
