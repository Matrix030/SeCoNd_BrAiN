---
tags: [book, os, operating-systems, book-os-concepts, scheduling]
aliases: ["Simulations"]
---

# Simulations

> [!note]
> [TODO - wtf]

1) To get a more accurate evaluation of scheduling algorithms, we can use **simulations**. Running simulations involves programming a model of the computer system. 
2) Software data structures represent the major components of the system. The simulator has a variable representing a clock; as this variable’s value is increased, the simulator modifies the system state to reflect the activities of the devices, the processes, and the scheduler. As the simulation executes, statistics that indicate algorithm performance are gathered and printed.
3) The data to drive the simulation can be generated in several ways. The most common method uses a random-number generator that is programmed to generate processes, CPU burst times, arrivals, departures, and so on, according to probability distributions. 
4) The distributions can be defined mathematically (uniform, exponential, Poisson) or empirically. If a distribution is to be defined empirically, measurements of the actual system under study are taken. 
5) The results define the distribution of events in the real system; this distribution can then be used to drive the simulation.
6) A distribution-driven simulation may be inaccurate, however, because of relationships between successive events in the real system. The frequency distribution indicates only how many instances of each event occur; it does not indicate anything about the order of their occurrence. To correct this problem, we can use **trace tapes**. We create a trace tape by monitoring the real system and recording the sequence of actual events (see figure below).
7) We then use this sequence to drive the simulation. Trace tapes provide an excellent way to compare two algorithms on exactly the same set of real inputs. This method can produce accurate results for its inputs.

![[Pasted image 20260611155036.png]]

8) Simulations can be expensive, often requiring hours of computer time. A more detailed simulation provides more accurate results, but it also takes more computer time. In addition, trace tapes can require large amounts of storage space. Finally, the design, coding, and debugging of the simulator can be a major task.

## Related

- [[> Algorithm Evaluation]] — back to the sub-topic MOC
- [[Queueing Models]] — the analytic alternative
- [[Implementation]] — the only fully accurate evaluation method
