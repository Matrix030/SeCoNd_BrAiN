---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["2.6.3"]
---

# Implementation

1) Once an operating system is designed, it must be implemented.
2) Traditionally, operating systems have been written in assembly language. Now, however, they are most commonly written in higher-level languages such as C or C++.
3) As is true in other systems, major performance improvements in operating systems are more likely to be the result of better data structures and algorithms than of excellent assembly-language code. In addition, although operating systems are large, only a small amount of the code is critical to high performance; the memory manager and the CPU scheduler are probably the most critical routines. After the system is written and is working correctly, bottleneck routines can be identified and can be replaced with assembly-language equivalents.

## Related

- [[> Operating-System Design and Implementation]] — back to the sub-topic MOC
- [[Mechanisms and Policies]] — the previous design concern
- [[Operating-System Design and Implementation]] — the section intro
