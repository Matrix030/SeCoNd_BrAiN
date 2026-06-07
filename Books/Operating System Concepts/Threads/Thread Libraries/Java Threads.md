---
tags: [book, os, operating-systems, book-os-concepts, processes, threads]
aliases: ["4.3.3"]
---

# Java Threads

1) Threads are the fundamental model of program execution in a Java program, and the Java language and its API provide a rich set of features for the creation and management of threads. All Java programs comprise at least a single thread of control—even a simple Java program consisting of only a main() method runs as a single thread in the JVM.
2) There are two techniques for creating threads in a Java program. One approach is to create a new class that is derived from the Thread class and to override its run() method. An alternative—and more commonly used—technique is to define a class that implements the Runnable interface. The Runnable interface is defined as follows:

```java
public interface Runnable
{
	public abstract void run();
}
```

3) When a class implements Runnable, it must define a run() method. The code implementing the run() method is what runs as a separate thread.
4) The program below shows the Java version of a multithreaded program that determines the summation of a non-negative integer. The Summation class implements the Runnable interface. 
5) Thread creation is performed by creating an object instance of the Thread class and passing the constructor a Runnable object.

```java
class Sum
{
	private int sum;

	public int getSum() {
		return sum;
	}

	public void setSum(int sum) {
		this.sum = sum;
	}
}

class Summation implements Runnable
{
	private int upper;
	private Sum sumValue;

	public Summation(int upper, Sum sumValue) {
		this.upper = upper;
		this.sumValue = sumValue;
	}

	public void run() {
		int sum = 0;
		for (int i = 0; i <= upper; i++)
			sum += i;
		sumValue.setSum(sum);
	}
}

public class Driver
{
	public static void main(String[] args) {
		if (args.length > 0) {
			if (Integer.parseInt(args[0]) < 0)
				System.err.println(args[0] + " must be >= 0.");
			else {
				// create the object to be shared
				Sum sumObject = new Sum();
				int upper = Integer.parseInt(args[0]);
				Thread thrd = new Thread(new Summation(upper, sumObject));
				thrd.start();
				try {
					thrd.join();
					System.out.println
						("The sum of "+upper+" is "+sumObject.getSum());
				} catch (InterruptedException ie) { }
			}
		}
		else
			System.err.println("Usage: Summation <integer value>");
	}
}
```

6) Creating a Thread object does not specifically create the new thread; rather, it is the start() method that creates the new thread. Calling the start() method for the new object does two things:
	1. It allocates memory and initializes a new thread in the JVM.
	2. It calls the run() method, making the thread eligible to be run by the JVM. (Note that we never call the run() method directly. Rather, we call the start() method, and it calls the run() method on our behalf.)
7) When the summation program runs, two threads are created by the JVM. The first is the parent thread, which starts execution in the main() method. 
8) The second thread is created when the start() method on the Thread object is invoked. This child thread begins execution in the run() method of the Summation class. 
9) After outputting the value of the summation, this thread terminates when it exits from its run() method.
10) Sharing of data between threads occurs easily in Win32 and Pthreads, since shared data are simply declared globally. As a pure object-oriented language, Java has no such notion of global data; if two or more threads are to share data in a Java program, the sharing occurs by passing references to the shared object to the appropriate threads. 
11) In the Java program shown above, the main thread and the summation thread share the object instance of the Sum class. 
12) This shared object is referenced through the appropriate getSum() and setSum() methods. (You might wonder why we don’t use an Integer object rather than designing a new sum class. The reason is that the Integer class is **immutable**—that is, once its value is set, it cannot change.)
13) Recall that the parent threads in the [[Pthreads]] and [[Win32 Threads]] libraries use pthread_join() and WaitForSingleObject() (respectively) to wait for the summation threads to finish before proceeding. The join() method in Java provides similar functionality. (Notice that join() can throw an InterruptedException, which we choose to ignore.)

## Related

- [[> Thread Libraries]] — back to the sub-topic MOC
- [[Pthreads]] — the POSIX equivalent
- [[Win32 Threads]] — the Windows equivalent
- [[Thread Library Implementation]] — the section preamble
