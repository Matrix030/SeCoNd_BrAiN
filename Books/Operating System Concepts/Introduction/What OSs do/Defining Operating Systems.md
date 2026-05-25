---
tags: [book, os, operating-systems, book-os-concepts]
aliases: ["Defining Operating Systems", "Kernel"]
---

# Defining Operating Systems

1) In general, we have no completely adequate definition of an operating system.
2) Operating systems exist because they offer a reasonable way to solve the problem of creating a usable computing system. 
3) The fundamental goal of computer systems is to execute user programs and to make solving user problems easier. 
4) Toward this goal, computer hardware is constructed. 
5) Since bare hardware alone is not particularly easy to use, application programs are developed. 
6) These programs require certain common operations, such as those controlling the I/O devices. 
7) The common functions of controlling and allocating resources are then brought together into one piece of software: **the operating system.**

> [!info] Storage Definitions and Notation
> 1) A **bit** is the basic unit of computer storage.
> 2) It can contain one of two values, zero and one. All other storage in a computer is based on collections of bits. 
> 3) Given enough bits, it is amazing how many things a computer can represent: numbers, letters, images, movies, sounds, documents, and programs, to name a few. 
> 4) A **byte** is 8 bits, and on most computers it is the smallest convenient chunk of storage.
> 5) For example, most computers don’t have an instruction to move a bit but do have one to move a byte. 
> 6) A less common term is **word**, which is a given computer architecture’s native storage unit. A word is generally made up of one or more bytes. For example, a computer may have instructions to move 64-bit (8-byte) words. A kilobyte, or KB, is 1,024 bytes; a megabyte, or MB, is 1,0242 bytes; and a gigabyte, or GB, is 1,0243 bytes. Computer manufacturers often round off these numbers and say that a megabyte is 1 million bytes and a gigabyte is 1 billion bytes.

7) A more common definition, and the one that we usually follow, is that the operating system is the one program running at all times on the computer—usually called the **kernel**. (Along with the kernel, there are two other types of programs: **systems programs**, which are associated with the operating system but are not part of the kernel, and **application programs**, which include all programs not associated with the operation of the system.)

## Related

- [[> What OSs do]] — back to the sub-topic MOC
- [[System View]] — the OS as resource allocator and control program
- [[Computer System Components]] — the four components an OS sits among
