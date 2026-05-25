## **1.2.1 Computer-System Operation**
1) A modern general-purpose computer system consists of one or more CPUs and a number of device controllers connected through a common bus that provides access to shared memory. 
2) Each device controller is in charge of a specific type of device (for example, disk drives, audio devices, and video displays).
3) The CPU and the device controllers can execute concurrently, competing for memory cycles. 
4) To **ensure orderly access to the shared memory**, a **memory controller** is provided whose **function is to synchronize access to the memory**.
		[TODO - wtf, how everything does from powering to final OS boot]
	1) For a computer to start running—for instance, when it is powered up or rebooted—it needs to have an initial program to run.
	2) This initial program, or **bootstrap program**, tends to be simple. 
	3) Typically, it is stored in read-only memory (**ROM**) or electrically erasable programmable read-only memory (**EEPROM**), known by the general term **firmware**, within the computer hardware. 
	4) It initializes all aspects of the system, from CPU registers to device controllers to memory contents. 
	5) The bootstrap program must know how to load the operating system and how to start executing that system. 
	6) To accomplish this goal, the bootstrap program must locate and load into memory the operating-system kernel.
	7) The operating system then starts executing the first process, such as “init,” and waits for some event to occur.
	
![[Pasted image 20260525162750.png]]

5) The occurrence of an event is usually signaled by an **interrupt** from either the hardware or the software.
6) Hardware may trigger an interrupt at any time by sending a signal to the CPU, usually by way of the system bus. 
7) Software may trigger an interrupt by executing a special operation called a **system call** (also called a **monitor call**).
8) When the CPU is interrupted, it stops what it is doing and immediately transfers execution to a fixed location. The fixed location usually contains the starting address where the service routine for the interrupt is located.
9) The interrupt service routine executes; on completion, the CPU resumes the interrupted computation. A time line of this operation is shown in the figure below.
10) **Interrupts** are an important part of a computer architecture. 
11) Each computer design has its own interrupt mechanism, but several functions are common. 
12) The interrupt must transfer control to the appropriate interrupt service routine.
13) The straightforward method for handling this transfer would be to invoke a generic routine to examine the interrupt information; the routine, in turn, would call the interrupt-specific handler.
14) However, interrupts must be handled quickly. Since only a predefined number of interrupts is possible, a table of pointers to interrupt routines can be used instead to provide the necessary speed. 
15) The interrupt routine is called indirectly through the table, with no intermediate routine needed. 
16) Generally, the table of pointers is stored in low memory (the first hundred or so locations).
17) These locations hold the addresses of the interrupt service routines for the various devices. 
18) This array, or **interrupt vector**, of addresses is then indexed by a unique device number, given with the interrupt request, to provide the address of the interrupt service routine for the interrupting device. 
19) Operating systems as different as Windows and UNIX dispatch interrupts in this manner.

![[Pasted image 20260525162945.png]]

16) The interrupt architecture must also save the address of the interrupted instruction. 
17) Many old designs simply stored the interrupt address in a fixed location or in a location indexed by the device number. 
18) More recent architectures store the return address on the system stack. 
19) If the interrupt routine needs to modify the processor state—for instance, by modifying register values—it must explicitly save the current state and then restore that state before returning. 
20) After the interrupt is serviced, the saved return address is loaded into the program counter, and the interrupted computation resumes as though the interrupt had not occurred.

## **1.2.2 Storage Structure**
1) The CPU can load instructions only from memory, so any programs to run must be stored there. 
2) General-purpose computers run most of their programs from rewriteable memory, called main memory (also called **random-access memory** or **RAM**).
3) Main memory commonly is implemented in a semiconductor technology called **dynamic random-access memory (DRAM)**. 
4) Because the read-only memory (ROM) cannot be changed, only static programs are stored there. The immutability of ROM is of use in game cartridges. 
5) EEPROM cannot be changed frequently and so contains mostly static programs.
6) For example, smartphones have EEPROM to store their factory-installed programs.
7) All forms of memory provide an array of words. Each word has its own address. 
8) Interaction is achieved through a sequence of load or store instructions to specific memory addresses. 
[TODO - wtf]
9) The load instruction moves a word from main memory to an internal register within the CPU, whereas the store instruction moves the content of a register to main memory. 
10) Aside from explicit loads and stores, the CPU automatically loads instructions from main memory for execution.
	1) A typical instruction-execution cycle, as executed on a system with a von **Neumann** architecture, first fetches an instruction from memory and stores that instruction in the **instruction register**. 
	2) The instruction is then decoded and may cause operands to be fetched from memory and stored in some internal register. 
	3) After the instruction on the operands has been executed, the result may be stored back in memory. 
	[!TODO - wtf]
	4) Notice that the memory unit sees only a stream of memory addresses; it does not know how they are generated (by the instruction counter, indexing, indirection, literal addresses, or some other means) or what they are for (instructions or data).
	5) Accordingly, we can ignore _how_ a memory address is generated by a program. We are interested only in the sequence of memory addresses generated by the running program.
11) Ideally, we want the programs and data to reside in main memory permanently. This arrangement usually is not possible for the following two reasons:
	- Main memory is usually too small to store all needed programs and data permanently.
	- Main memory is a _volatile_ storage device that loses its contents when power is turned off or otherwise lost.
12) Thus, most computer systems provide **secondary storage** as an extension of main memory. The main requirement for secondary storage is that it be able to hold large quantities of data permanently.
13) The most common secondary-storage device is a **magnetic disk**, which provides storage for both programs and data.
14) The wide variety of storage systems in a computer system can be organized in a hierarchy according to speed and cost. The higher levels are expensive, but they are fast. 
15) As we move down the hierarchy, the cost per bit generally decreases, whereas the access time generally increases. This trade-off is reasonable; if a given storage system were both faster and less expensive than another—other properties being the same—then there would be no reason to use the slower, more expensive memory. 
16) In addition to differing in speed and cost, the various storage systems are either volatile or nonvolatile. 
	- As mentioned earlier, **volatile storage** loses its contents when the power to the device is removed.
	- In the absence of expensive battery and generator backup systems, data must be written to **nonvolatile storage** for safekeeping. The storage systems above the electronic disk are volatile, whereas those below are nonvolatile.
![[Pasted image 20260525165531.png]]

## **1.2.3 I/O Structure**
1) A large portion of operating system code is dedicated to managing I/O, both because of its importance to the reliability and performance of a system and because of the varying nature of the devices. 
2) A general-purpose computer system consists of CPUs and multiple device controllers that are connected through a common bus. 
3) Each device controller is in charge of a specific type of device. 
4) Depending on the controller, more than one device may be attached. 
5) For instance, seven or more devices can be attached to the **small computer-systems interface (SCSI)** controller. 
6) A device controller maintains some local buffer storage and a set of special-purpose registers. The device controller is responsible for moving the data between the peripheral devices that it controls and its local buffer storage. Typically, operating systems have a **device driver** for each device controller. This device driver understands the device controller and presents a uniform interface to the device to the rest of the operating system.

To start an I/O operation, the device driver loads the appropriate registers within the device controller. The device controller, in turn, examines the contents of these registers to determine what action to take (such as “read a character from the keyboard”). The controller starts the transfer of data from the device to its local buffer. Once the transfer of data is complete, the device controller informs the device driver via an interrupt that it has finished its operation. The device driver then returns control to the operating system, possibly returning the data or a pointer to the data if the operation was a read. For other operations, the device driver returns status information.

This form of interrupt-driven I/O is fine for moving small amounts of data but can produce high overhead when used for bulk data movement such as disk I/O. To solve this problem, **direct memory access (DMA)** is used. After setting up buffers, pointers, and counters for the I/O device, the device controller transfers an entire block of data directly to or from its own buffer storage to memory, with no intervention by the CPU. Only one interrupt is generated per block, to tell the device driver that the operation has completed, rather than the one interrupt per byte generated for low-speed devices. While the device controller is performing these operations, the CPU is available to accomplish other work.

Some high-end systems use switch rather than bus architecture. On these systems, multiple components can talk to other components concurrently, rather than competing for cycles on a shared bus. In this case, DMA is even more effective. [Figure 1.5](https://learning.oreilly.com/library/view/operating-system-concepts/9780470128725/silb_9780470128725_oeb_c01_r1.html#FIG-1.5-section-1-5-1-3-1) shows the interplay of all components of a computer system.