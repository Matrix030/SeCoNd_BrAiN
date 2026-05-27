---
tags: [book, os, operating-systems, book-os-concepts, processes]
aliases: ["Real-Time Embedded Systems", "Real-Time Operating Systems"]
---

# Real-Time Embedded Systems

1) Embedded computers are the most prevalent form of computers in existence. They tend to have very specific tasks (robots, DVDs, microwave ovens). The systems they run on are usually primitive, and so their OSs have limited features.
2) Some are general-purpose computers, running standard operating systems—such as UNIX—with special-purpose applications to implement the functionality. Others are hardware devices with a special-purpose embedded operating system providing just the functionality desired. Yet others are hardware devices with application-specific integrated circuits (**ASICs**) that perform their tasks without an operating system.
3) Embedded systems almost always run **real-time operating systems**. A real-time system is used when rigid time requirements have been placed on the operation of a processor or the flow of data; thus, it is often used as a control device in a dedicated application. Sensors bring data to the computer. The computer must analyze the data and possibly adjust controls to modify the sensor inputs. 
4) A real-time system has well-defined, fixed time constraints. Processing _must_ be done within the defined constraints, or the system will fail. For instance, it would not do for a robot arm to be instructed to halt _after_ it had smashed into the car it was building. A real-time system functions correctly only if it returns the correct result within its time constraints. Contrast this system with a time-sharing system, where it is desirable (but not mandatory) to respond quickly, or a batch system, which may have no time constraints at all.

## Related

- [[> Special-Purpose Systems]] — back to the sub-topic MOC
- [[Multimedia Systems]] — another class of special-purpose system
- [[Handheld Systems]] — devices that often run embedded operating systems
