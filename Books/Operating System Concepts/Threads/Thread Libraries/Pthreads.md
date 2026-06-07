---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.3.1"]
---

# Pthreads

1) **Pthreads** refers to the POSIX standard (IEEE 1003.1c) defining an API for thread creation and synchronization. 
2) This is a _specification_ for thread behavior, not an _implementation._ Operating system designers may implement the specification in any way they wish.
3) The C program shown below demonstrates the basic Pthreads API for constructing a multithreaded program that calculates the summation of a non-negative integer in a separate thread. 
4) In a Pthreads program, separate threads begin execution in a specified function. In this program, this is the runner() function. When this program begins, a single thread of control begins in main(). After some initialization, main() creates a second thread that begins control in the runner() function. Both threads share the global data sum.
5) Let’s look more closely at this program. All Pthreads programs must include the pthread.h header file. The statement pthread_t tid declares the identifier for the thread we will create. Each thread has a set of attributes, including stack size and scheduling information. The pthread_attr_t attr declaration represents the attributes for the thread. We set the attributes in the function call pthread_attr_init(&attr). Because we did not explicitly set any attributes, we use the default attributes provided. (In Chapter 5, we discuss some of the scheduling attributes provided by the Pthreads API.) A separate thread is created with the pthread_create() function call. In addition to passing the thread identifier and the attributes for the thread, we also pass the name of the function where the new thread will begin execution—in this case, the runner() function. Last, we pass the integer parameter that was provided on the command line, argv[1].

At this point, the program has two threads: the initial (or parent) thread in main() and the summation (or child) thread performing the summation operation in the runner() function. After creating the summation thread, the parent thread will wait for it to complete by calling the pthread_join() function. The summation thread will complete when it calls the function pthread_exit(). Once the summation thread has returned, the parent thread will output the value of the shared data sum.

```c
#include <pthread.h>
#include <stdio.h>

int sum; /* this data is shared by the thread(s) */
void *runner(void *param); /* the thread */

int main(int argc, char *argv[])
{
	pthread_t tid; /* the thread identifier */
	pthread_attr_t attr; /* set of thread attributes */

	if (argc != 2) {
		fprintf(stderr,"usage: a.out <integer value>\n");
		return -1;
	}
	if (atoi(argv[1]) < 0) {
		fprintf(stderr,"%d must be >= 0\n",atoi(argv[1]));
		return -1;
	}

	/* get the default attributes */
	pthread_attr_init(&attr);
	/* create the thread */
	pthread_create(&tid,&attr,runner,argv[1]);
	/* wait for the thread to exit */
	pthread_join(tid,NULL);

	printf("sum = %d\n",sum);
}

/* The thread will begin control in this function */
void *runner(void *param)
{
	int i, upper = atoi(param);
	sum = 0;

	for (i = 1; i <= upper; i++)
		sum += i;

	pthread_exit(0);
}
```

## Related

- [[> Thread Libraries]] — back to the sub-topic MOC
- [[Thread Library Implementation]] — the section preamble
- [[Win32 Threads]] — the equivalent Windows API
- [[Java Threads]] — the equivalent Java API
