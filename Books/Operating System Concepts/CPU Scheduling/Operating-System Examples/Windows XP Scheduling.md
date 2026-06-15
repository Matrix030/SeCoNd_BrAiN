---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Example: Windows XP Scheduling"]
---

# Windows XP Scheduling

1) Windows XP schedules threads using a priority-based, preemptive scheduling algorithm. The Windows XP scheduler ensures that the highest-priority thread will always run. The portion of the Windows XP kernel that handles scheduling is called the _dispatcher_. A thread selected to run by the dispatcher will run until it is preempted by a higher-priority thread, until it terminates, until its time quantum ends, or until it calls a blocking system call, such as for I/O. If a higher-priority real-time thread becomes ready while a lower-priority thread is running, the lower-priority thread will be preempted. This preemption gives a real-time thread preferential access to the CPU when the thread needs such access.
2) The dispatcher uses a 32-level priority scheme to determine the order of thread execution. Priorities are divided into two classes. The **variable class** contains threads having priorities from 1 to 15, and the **real-time class** contains threads with priorities ranging from 16 to 31. (There is also a thread running at priority 0 that is used for memory management.) The dispatcher uses a queue for each scheduling priority and traverses the set of queues from highest to lowest until it finds a thread that is ready to run. If no ready thread is found, the dispatcher will execute a special thread called the **idle thread**.

![[Pasted image 20260611152354.png]]

3) There is a relationship between the numeric priorities of the Windows XP kernel and the Win32 API. The Win32 API identifies several priority classes to which a process can belong. These include:
	- REALTIME_PRIORITY_CLASS
	- HIGH_PRIORITY_CLASS
	- ABOVE_NORMAL_PRIORITY_CLASS
	- NORMAL_PRIORITY_CLASS

![[Pasted image 20260611152406.png]]

	- BELOW_NORMAL_PRIORITY_CLASS
	- IDLE_PRIORITY_CLASS
4) Priorities in all classes except the REALTIME_PRIORITY_CLASS are variable, meaning that the priority of a thread belonging to one of these classes can change.
5) A thread within a given priority classes also has a relative priority. The values for relative priorities include:
	- TIME_CRITICAL
	- HIGHEST
	- ABOVE_NORMAL
	- NORMAL
	- BELOW_NORMAL
	- LOWEST
	- IDLE
6) The priority of each thread is based on both the priority class it belongs to and its relative priority within that class. This relationship is shown in Figure 5.14. The values of the priority classes appear in the top row. The left column contains the values for the relative priorities. For example, if the relative priority of a thread in the ABOVE_NORMAL_PRIORITY_CLASS is NORMAL, the numeric priority of that thread is 10.
7) Furthermore, each thread has a base priority representing a value in the priority range for the class the thread belongs to. By default, the base priority is the value of the NORMAL relative priority for that class. The base priorities for each priority class are:
	- REALTIME_PRIORITY_CLASS—24
	- HIGH_PRIORITY_CLASS—13
	- ABOVE_NORMAL_PRIORITY_CLASS—10
	- NORMAL_PRIORITY_CLASS—8
	- BELOW_NORMAL_PRIORITY_CLASS—6
	- IDLE_PRIORITY_CLASS—4
8) Processes are typically members of the NORMAL_PRIORITY_CLASS. A process belongs to this class unless the parent of the process was of the IDLE_PRIORITY_CLASS or unless another class was specified when the process was created. The initial priority of a thread is typically the base priority of the process the thread belongs to.
9) When a thread’s time quantum runs out, that thread is interrupted; if the thread is in the variable-priority class, its priority is lowered. The priority is never lowered below the base priority, however. Lowering the priority tends to limit the CPU consumption of compute-bound threads. When a variable-priority thread is released from a wait operation, the dispatcher boosts the priority. The amount of the boost depends on what the thread was waiting for; for example, a thread that was waiting for keyboard I/O would get a large increase, whereas a thread waiting for a disk operation would get a moderate one. This strategy tends to give good response times to interactive threads that are using the mouse and windows. It also enables I/O-bound threads to keep the I/O devices busy while permitting compute-bound threads to use spare CPU cycles in the background. This strategy is used by several time-sharing operating systems, including UNIX. In addition, the window with which the user is currently interacting receives a priority boost to enhance its response time.
10) When a user is running an interactive program, the system needs to provide especially good performance. For this reason, Windows XP has a special scheduling rule for processes in the NORMAL_PRIORITY_CLASS. Windows XP distinguishes between the _foreground process_ that is currently selected on the screen and the _background processes_ that are not currently selected. When a process moves into the foreground, Windows XP increases the scheduling quantum by some factor—typically by 3. This increase gives the foreground process three times longer to run before a time-sharing preemption occurs.
