---
tags: [book, os, operating-systems, book-os-concepts, concurrency]
aliases: ["Process Synchronization Introduction"]
---

# Process Synchronization

1) A cooperating process is one that can affect or be affected by other processes executing in the system. Cooperating processes can either directly share a logical address space (that is, both code and data) or be allowed to share data only through files or messages. Concurrent access to shared data may result in data inconsistency, however. In this chapter, we discuss various mechanisms to ensure the orderly execution of cooperating processes that share a logical address space, so that data consistency is maintained.

CHAPTER OBJECTIVES
- To introduce the critical-section problem, whose solutions can be used to ensure the consistency of shared data.
- To present both software and hardware solutions of the critical-section problem.
- To introduce the concept of an atomic transaction and describe mechanisms to ensure atomicity.

## Related

- [[> Process Synchronization]] — back to the chapter MOC
- [[> Processes]] — the cooperating-process model this chapter builds on
- [[> Background]] — why concurrent access to shared data needs synchronization
