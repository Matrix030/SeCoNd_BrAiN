---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["2.4.1"]
---

# Process Control

3) If Program A starts Program B:  
	- Either A is replaced entirely by B  
	- Or A is paused and restored later  
	- (like [[Command Interpreter|shell]] -> vim -> shell)  
	- Or both A and B run together  
		(modern multitasking)
![[Pasted image 20260528145758.png]]
4) If we create a new job or [[process]], or perhaps even a set of jobs or processes, we should be able to control its execution. This control requires the ability to determine and reset the attributes of a job or process, including the job’s priority, its maximum allowable execution time, and so on (get process attributes and set process attributes). We may also want to terminate a job or process that we created (terminate process) if we find that it is incorrect or is no longer needed.
5) Having created new jobs or processes, we may need to wait for them to finish their execution. We may want to wait for a certain amount of time to pass (wait time); more probably, we will want to wait for a specific event to occur (wait event). The jobs or processes should then signal when that event has occurred (signal event). Quite often, two or more processes may share data. To ensure the integrity of the data being shared, operating systems often provide system calls allowing a process to **lock** shared data, thus preventing another process from accessing the data while it is locked. Typically such system calls include acquire lock and release lock.

**EXAMPLE OF STANDARD C LIBRARY**
1) The standard C library provides a portion of the [[System Calls|system-call interface]] for many versions of UNIX and Linux. As an example, let’s assume a C program invokes the printf() statement. 
	- The C library intercepts this call and invokes the necessary [[system call]](s) in the operating system—in this instance, the write() system call.
	- The C library takes the value returned by write() and passes it back to the user program. This is shown in the Figure below.
![[Pasted image 20260528145816.png]]

2) Because MS-DOS is single-tasking, it uses a simple method to run a program and does not create a new process. 
	- It loads the program into memory, writing over most of itself to give the program as much memory as possible (see figure below). 
	- Next, it sets the instruction pointer to the first instruction of the program. 
	- The program then runs, and either an error causes a trap, or the program executes a system call to terminate. 
	- In either case, the error code is saved in the system memory for later use. 
	- Following this action, the small portion of the [[Command Interpreter|command interpreter]] that was not overwritten resumes execution. 
	- Its first task is to reload the rest of the command interpreter from disk. 
	- Then the command interpreter makes the previous error code available to the user or to the next program.
![[Pasted image 20260528145843.png]]

3) FreeBSD (derived from Berkeley UNIX) is an example of a multitasking system. 
	- When a user logs on to the system, the shell of the user’s choice is run.
	- This shell is similar to the MS-DOS shell in that it accepts commands and executes programs that the user requests.
	- However, since FreeBSD is a multitasking system, the command interpreter may continue running while another program is executed (see figure below). 
	- To start a new process, the shell executes a **fork()** system call. Then, the selected program is loaded into memory via an exec() system call, and the program is executed. 
	- Depending on the way the command was issued, the shell then either waits for the process to finish or runs the process “in the background.” 
	- In the latter case, the shell immediately requests another command. When a process is running in the background, it cannot receive input directly from the keyboard, because the shell is using this resource.
	- I/O is therefore done through files or through a GUI interface. 
	- Meanwhile, the user is free to ask the shell to run other programs, to monitor the progress of the running process, to change that program’s priority, and so on. 
	- When the process is done, it executes an exit() system call to terminate, returning to the invoking process a status code of 0 or a nonzero error code.
	- This status or error code is then available to the shell or other programs. 
	- Processes are discussed in Chapter 3 with a program example using the fork() and exec() system calls.

![[Pasted image 20260528145855.png]]

## Related

- [[> Types of System Calls]] — back to the sub-topic MOC
- [[Types of System Calls]] — the six categories of system calls
- [[File Management]] — the next category of system calls
- [[Command Interpreter]] — the shell that issues process-control calls
- [[process]] — the unit of execution being controlled
