---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.8.3"]
---

# Simulation

1) Another methodology is **simulation**, in which the host system has one system architecture and the guest system was compiled for a different architecture.
2) Emulation can increase the life of programs and allow us to explore old architectures without having an actual old machine, but its major challenge is performance.
3) Instruction-set emulation can run an order of magnitude slower than native instructions. 
4) Thus, unless the new machine is ten times faster than the old, the program running on the new machine will run slower than it did on its native hardware. 
5) Another challenge is that it is difficult to create a correct emulator because, in essence, this involves writing an entire CPU in software.

## Related

- [[> Virtual Machines]] — back to the sub-topic MOC
- [[Para-virtualization]] — a different virtualization approach
- [[Implementation]] — implementing virtual machines
