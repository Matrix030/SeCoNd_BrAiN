---
tags: [book, os, operating-systems, book-os-concepts, processes, scheduling]
aliases: ["3.2.1"]
---

# Scheduling Queues

1) As processes enter the system, they are put into a **job queue**, which consists of all processes in the system. 
2) The processes that are residing in main memory and are ready and waiting to execute are kept on a list called the **ready queue**.

**PROCESS REPRESENTATION IN LINUX**
1) The process control block in the Linux operating system is represented by the C structure task_struct. This structure contains all the necessary information for representing a process, including:
	- the state of the process,
	- scheduling and memory-management information,
	- list of open files, and 
	- pointers to the process’s parent and any of its children. (A process’s _parent_ is the process that created it; its _children_ are any processes that it creates.) Some of these fields include:

![[Pasted image 20260601212941.png]]

2) For example, the state of a process is represented by the field long state in this structure.
3) Within the Linux kernel, all active processes are represented using a doubly linked list of task_struct, and the kernel maintains a pointer —current—to the process currently executing on the system. This is shown in [Figure 3.5](https://learning.oreilly.com/library/view/operating-system-concepts/9780470128725/silb_9780470128725_oeb_c03_r1.html#FIG-3.5-section-1-6-1-2-1).

![[Pasted image 20260601212953.png]]

4) As an illustration of how the kernel might manipulate one of the fields in the task_struct for a specified process, let’s assume the system would like to change the state of the process currently running to the value new_state. 
5) If current is a pointer to the process currently executing, its state is changed with the following:

![[Pasted image 20260601213004.png]]

6) This queue is generally stored as a linked list.
7) A ready-queue header contains pointers to the first and final [[Process Control Block|PCBs]] in the list. 
8) Each PCB includes a pointer field that points to the next PCB in the ready queue.

![[Pasted image 20260601213015.png]]

9) The system also includes other queues. When a process is allocated the CPU, it executes for a while and eventually quits, is interrupted, or waits for the occurrence of a particular event, such as the completion of an I/O request. 
10) Suppose the process makes an I/O request to a shared device, such as a disk. Since there are many processes in the system, the disk may be busy with the I/O request of some other process. 
11) The process therefore may have to wait for the disk. The list of processes waiting for a particular I/O device is called a **device queue**. Each device has its own device queue(see figure above).
12) A common representation of process scheduling is a **queueing diagram**, such as that in the figure below. Each rectangular box represents a queue.
13) Two types of queues are present: the ready queue and a set of device queues. The circles represent the resources that serve the queues, and the arrows indicate the flow of processes in the system.
14) A new process is initially put in the ready queue. It waits there until it is selected for execution, or is **dispatched**. 
15) Once the process is allocated the CPU and is executing, one of several events could occur:
	- The process could issue an I/O request and then be placed in an I/O queue.
	- The process could create a new subprocess and wait for the subprocess’s termination.
	- The process could be removed forcibly from the CPU, as a result of an interrupt, and be put back in the ready queue.

![[Pasted image 20260601213032.png]]

16) In the first two cases, the process eventually switches from the [[Process State|waiting state to the ready state]] and is then put back in the ready queue. A process continues this cycle until it terminates, at which time it is removed from all queues and has its PCB and resources deallocated.

## Related

- [[> Process Scheduling]] — back to the sub-topic MOC
- [[Schedulers]] — the schedulers that move processes between these queues
- [[Process Control Block]] — the PCB linked into the ready and device queues
- [[Process State]] — the states a process passes through as it moves between queues
