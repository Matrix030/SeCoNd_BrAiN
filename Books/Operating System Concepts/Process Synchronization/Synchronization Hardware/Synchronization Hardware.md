---
tags: [book, os, operating-systems, book-os-concepts, concurrency]
aliases: ["Synchronization Hardware"]
---

# Synchronization Hardware

1) We have just described one software-based solution to the critical-section problem. However, as mentioned, software-based solutions such as [[Peterson's Solution|Peterson's]] are not guaranteed to work on modern computer architectures. 
2) Instead, we can generally state that any solution to the critical-section problem requires a simple tool—a **lock**. Race conditions are prevented by requiring that critical regions be protected by locks. 
3) That is, a process must acquire a lock before entering a critical section; it releases the lock when it exits the critical section. This is illustrated in the figure below.

**Figure 6.3** Solution to the critical-section problem using locks.

```
do {

    acquire lock

        critical section

    release lock

        remainder section

} while (TRUE);
```

4) We start by presenting some simple hardware instructions that are available on many systems and showing how they can be used effectively in solving the critical-section problem. Hardware features can make any programming task easier and improve system efficiency.
5) Many modern computer systems provide special hardware instructions that allow us either to test and modify the content of a word or to swap the contents of two words **atomically**—that is, as one uninterruptible unit. We can use these special instructions to solve the critical-section problem in a relatively simple manner. Rather than discussing one specific instruction for one specific machine, we abstract the main concepts behind these types of instructions by describing the TestAndSet() and Swap() instructions.
6) The TestAndSet() instruction can be defined as shown in figure 6.4. The important characteristic of this instruction is that it is executed atomically. Thus, if two TestAndSet() instructions are executed simultaneously (each on a different CPU), they will be executed sequentially in some arbitrary order. If the machine supports the TestAndSet() instruction, then we can implement mutual exclusion by declaring a Boolean variable lock, initialized to false. The structure of process _P__i_ is shown in Figure 6.5.

**Figure 6.4** The definition of the TestAndSet() instruction.

```c
boolean TestAndSet(boolean *target) {
    boolean rv = *target;
    *target = TRUE;
    return rv;
}
```

**Figure 6.5** Mutual-exclusion implementation with TestAndSet().

```c
do {
    while (TestAndSet(&lock))
        ; // do nothing

        // critical section

    lock = FALSE;

        // remainder section
} while (TRUE);
```

7) The Swap() instruction, in contrast to the TestAndSet() instruction, operates on the contents of two words; it is defined as shown in Figure 6.6. Like the TestAndSet() instruction, it is executed atomically. If the machine supports the Swap() instruction, then mutual exclusion can be provided as follows. 
8) A global Boolean variable lock is declared and is initialized to false. In addition, each process has a local Boolean variable key. The structure of process _P__i_ is shown in [Figure 6.7](https://learning.oreilly.com/library/view/operating-system-concepts/9780470128725/silb_9780470128725_oeb_c06_r1.html#FIG-6.7-section-1-6-4-4).

**Figure 6.6** The definition of the Swap() instruction.

```c
void Swap(boolean *a, boolean *b) {
    boolean temp = *a;
    *a = *b;
    *b = temp;
}
```

**Figure 6.7** Mutual-exclusion implementation with the Swap() instruction.

```c
do {
    key = TRUE;
    while (key == TRUE)
        Swap(&lock, &key);

        // critical section

    lock = FALSE;

        // remainder section
} while (TRUE);
```

9) Although these algorithms satisfy the mutual-exclusion requirement, they do not satisfy the bounded-waiting requirement. In Figure 6.8, we present another algorithm using the TestAndSet() instruction that satisfies all the critical-section requirements. The common data structures are

```c
boolean waiting[n];
boolean lock;
```

10) These data structures are initialized to false. To prove that the mutual-exclusion requirement is met, we note that process _P__i_ can enter its critical section only if either waiting[i] == false or key == false. 
11) The value of key can become false only if the TestAndSet() is executed. The first process to execute the TestAndSet() will find key == false; all others must wait. The variable waiting[i] can become false only if another process leaves its critical section; only one waiting[i] is set to false, maintaining the mutual-exclusion requirement.

**Figure 6.8** Bounded-waiting mutual exclusion with TestAndSet().

```c
do {
    waiting[i] = TRUE;
    key = TRUE;
    while (waiting[i] && key)
        key = TestAndSet(&lock);
    waiting[i] = FALSE;

        // critical section

    j = (i + 1) % n;
    while ((j != i) && !waiting[j])
        j = (j + 1) % n;

    if (j == i)
        lock = FALSE;
    else
        waiting[j] = FALSE;

        // remainder section
} while (TRUE);
```

12) To prove that the progress requirement is met, we note that the arguments presented for mutual exclusion also apply here, since a process exiting the critical section either sets lock to false or sets waiting[j] to false. Both allow a process that is waiting to enter its critical section to proceed.
13) To prove that the bounded-waiting requirement is met, we note that, when a process leaves its critical section, it scans the array waiting in the cyclic ordering (_i_ + 1, _i_ + 2, ..., _n_ - 1, 0, ..., _i_ - 1). It designates the first process in this ordering that is in the entry section (waiting[j] == true) as the next one to enter the critical section. Any process waiting to enter its critical section will thus do so within _n_ - 1 turns.

> [!warning]
> Unfortunately for hardware designers, implementing atomic TestAndSet () instructions on multiprocessors is not a trivial task.

## Related

- [[> Synchronization Hardware]] — back to the sub-topic MOC
- [[Peterson's Solution]] — the software solution this section improves upon
- [[The Critical-Section Problem]] — the mutual-exclusion, progress, and bounded-waiting requirements these instructions satisfy
