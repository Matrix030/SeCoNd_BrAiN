---
tags: [book, os, operating-systems, book-os-concepts, concurrency]
aliases: ["Background"]
---

# Background

1) Let’s return to our consideration of the bounded buffer. As we pointed out, our original solution allowed at most BUFFER_SIZE—1 items in the buffer at the same time. Suppose we want to modify the algorithm to remedy this deficiency. One possibility is to add an integer variable counter, initialized to 0. counter is incremented every time we add a new item to the buffer and is decremented every time we remove one item from the buffer. The code for the producer process can be modified as follows:

![[Pasted image 20260611162201.png]]

The code for the consumer process can be modified as follows:
![[Pasted image 20260611162213.png]]
2) Although both the producer and consumer routines shown above are correct separately, they may not function correctly when executed concurrently. As an illustration, suppose that the value of the variable counter is currently 5 and that the producer and consumer processes execute the statements “counter++” and “counter--” concurrently. 
3) Following the execution of these two statements, the value of the variable counter may be 4, 5, or 6! The only correct result, though, is counter == 5, which is generated correctly if the producer and consumer execute separately.
4) We can show that the value of counter may be incorrect as follows. Note that the statement “counter++” may be implemented in machine language (on a typical machine) as
```
	_register_0 = counter  
	_register_1 = _register_1 + 1  
	counter = _register_1
```

- where _register_1 is one of the local CPU registers. 
Similarly, the statement _register_2“counter--” is implemented as follows:

```
	_register_2 = counter  
	_register_2 = _register_2 - 1  
	counter = _register_2
```
- where again _register_2 is on eof the local CPU registers. Even though _register_1 and _register_2 may be the same physical register (an accumulator, say), remember that the contents of this register will be saved and restored by the interrupt handler (Section 1.2.3).

5) The concurrent execution of “counter++” and “counter--” is equivalent to a sequential execution in which the lower-level statements presented previously are interleaved in some arbitrary order (but the order within each high-level statement is preserved). One such interleaving is

![[Pasted image 20260611162228.png]]

6) Notice that we have arrived at the incorrect state “counter == 4”, indicating that four buffers are full, when, in fact, five buffers are full. If we reversed the order of the statements at _T_4 and _T_5, we would arrive at the incorrect state “counter == 6”.
7) We would arrive at this incorrect state because we allowed both processes to manipulate the variable counter concurrently. A situation like this, where several processes access and manipulate the same data concurrently and the outcome of the execution depends on the particular order in which the access takes place, is called a **race condition**.
8) To guard against the race condition above, we need to ensure that only one process at a time can be manipulating the variable counter. To make such a guarantee, we require that the processes be synchronized in some way.
9) Situations such as the one just described occur frequently in operating systems as different parts of the system manipulate resources. Furthermore, with the growth of multicore systems, there is an increased emphasis on developing multithreaded applications wherein several threads—which are quite possibly sharing data—are running in parallel on different processing cores. 
10) Clearly, we want any changes that result from such activities not to interfere with one another. Because of the importance of this issue, a major portion of this chapter is concerned with **process synchronization** and **coordination** amongst cooperating processes.

## Related

- [[> Background]] — back to the sub-topic MOC
- [[> Process Synchronization]] — back to the chapter MOC
- [[Process Concept]] — the bounded-buffer producer–consumer model
