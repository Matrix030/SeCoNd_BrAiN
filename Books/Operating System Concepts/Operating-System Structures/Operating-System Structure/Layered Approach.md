---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.7.2"]
---

# Layered Approach

1) With proper hardware support, operating systems can be broken into pieces that are smaller and more appropriate than those allowed by the original MS-DOS and UNIX systems. 
2) The operating system can then retain much greater control over the computer and over the applications that make use of that computer. 
3) Implementers have more freedom in changing the inner workings of the system and in creating modular operating systems.
4) Under a top-down approach, the overall functionality and features are determined and are separated into components. Information hiding is also important, because it leaves programmers free to implement the low-level routines as they see fit, provided that the external interface of the routine stays unchanged and that the routine itself performs the advertised task.
5) A system can be made modular in many ways. One method is the **layered approach**, in which the operating system is broken into a number of layers (levels).
6) The bottom layer (layer 0) is the hardware; the highest (layer _N_) is the user interface. This layering structure is depicted in the figure below.

![[Pasted image 20260529212342.png]]

7) An operating-system layer is an implementation of an abstract object made up of data and the operations that can manipulate those data. A typical operating-system layer—say, layer _M_—consists of data structures and a set of routines that can be invoked by higher-level layers. Layer _M,_ in turn, can invoke operations on lower-level layers.
8) The main advantage of the layered approach is simplicity of construction and debugging. The layers are selected so that each uses functions (operations) and services of only lower-level layers. 
9) This approach simplifies debugging and system verification. The first layer can be debugged without any concern for the rest of the system, because, by definition, it uses only the basic hardware (which is assumed correct) to implement its functions.
10) Once the first layer is debugged, its correct functioning can be assumed while the second layer is debugged, and so on. 
11) If an error is found during the debugging of a particular layer, the error must be on that layer, because the layers below it are already debugged. Thus, the design and implementation of the system are simplified.
12) Each layer is implemented with only those operations provided by lower-level layers. A layer does not need to know how these operations are implemented; it needs to know only what these operations do. Hence, each layer hides the existence of certain data structures, operations, and hardware from higher-level layers.
### Challenges of the Layered OS Approach
- **Defining layers is difficult**
    - Each layer can only use services from lower layers.
    - Careful planning is needed to decide what belongs in each layer.
- **Dependencies can be complex**
    - Example: The **backing-store (disk) driver** must be below **memory management** because virtual memory relies on disk storage.
- **Some layer relationships are not obvious**
    - Normally, the backing-store driver should be above the **CPU scheduler** because I/O operations may block and require rescheduling.
    - However, if scheduler data itself must be swapped to disk, the backing-store driver must be below the CPU scheduler.
    - This creates design challenges and potential circular dependencies.
- **Performance overhead**
    - Every request passes through multiple layers.
    - Example for an I/O operation:
        - User Program
        - → I/O Layer
        - → Memory Management Layer
        - → CPU Scheduling Layer
        - → Hardware
    - Each layer may:
        - Modify parameters
        - Pass data
        - Perform additional checks
- **Result: Reduced efficiency**
    - More layer transitions mean more overhead.
    - System calls generally take longer than in non-layered operating systems.

## Related

- [[> Operating-System Structure]] — back to the sub-topic MOC
- [[Simple Structure]] — the unstructured monolithic alternative
- [[Microkernels]] — a different modularization strategy
- [[Modules]] — modular kernels that resemble a layered system
