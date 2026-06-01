---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.8.4"]
---

# Para-virtualization

1) Rather than try to trick a guest operating system into believing it has a system to itself, para-virtualization presents the guest with a system that is similar but not identical to the guest’s preferred system.
2) The guest must be modified to run on the paravirtualized hardware. The gain for this extra work is more efficient use of resources and a smaller virtualization layer.
3) Full virtualization hides the fact that a VM is virtual, while paravirtualization makes the guest OS aware that it is virtualized, allowing it to cooperate with the hypervisor for better performance.

## Related

- [[> Virtual Machines]] — back to the sub-topic MOC
- [[Simulation]] — the previous virtualization approach
- [[Implementation]] — implementing virtual machines
