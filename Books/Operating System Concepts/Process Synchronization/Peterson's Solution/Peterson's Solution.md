---
tags: [book, os, operating-systems, book-os-concepts, concurrency]
aliases: ["Peterson's Solution"]
---

# Peterson's Solution

## Overview

1) Next, we illustrate a classic software-based solution to the critical-section problem known as **Peterson's solution**. 
2) Because of the way modern computer architectures perform basic machine-language instructions, such as load and store, there are no guarantees that Peterson's solution will work correctly on such architectures.
3) However, we present the solution because it provides a good algorithmic description of solving the critical-section problem and illustrates some of the complexities involved in designing software that addresses the requirements of mutual exclusion, progress, and bounded waiting.
4) Peterson's solution is restricted to two processes that alternate execution between their critical sections and remainder sections. The processes are numbered _P_0 and _P_1. For convenience, when presenting _P__i_, we use _P__j_ to denote the other process; that is, j equals 1 - i.

## Shared data

Peterson's solution requires the two processes to share two data items:

![[Pasted image 20260611171040.png]]

- **`turn`** — indicates whose turn it is to enter its critical section. If `turn == i`, then process _P__i_ is allowed to execute in its critical section.
- **`flag[]`** — indicates whether a process *is ready* to enter its critical section. If `flag[i]` is true, then _P__i_ is ready to enter its critical section.

## The algorithm

![[Pasted image 20260611171051.png]]

**Entry protocol** for process _P__i_:

1. `flag[i] = true` — announce "I am ready to enter."
2. `turn = j` — hand the turn to the other process (assert that if it wants in, it may go).
3. `while (flag[j] && turn == j)` — busy-wait *only* while the other process is both ready **and** it is its turn.
4. Enter the **critical section**.

**Exit protocol** for process _P__i_:

5. `flag[i] = false` — announce "I am done", releasing the section.

> [!note] What happens when both try at once
> If both processes try to enter at the same time, `turn` will be set to both `i` and `j` at roughly the same time. Only one of these assignments will last; the other will occur but will be overwritten immediately. The eventual value of `turn` decides which process enters first.

## Correctness

We prove the solution satisfies all three requirements:

1. Mutual exclusion is preserved.
2. The progress requirement is satisfied.
3. The bounded-waiting requirement is met.

### Mutual exclusion

- Each _P__i_ enters its critical section only if either `flag[j] == false` or `turn == i`.
- For both processes to be in their critical sections at the same time, we would need `flag[0] == flag[1] == true`.
- But `turn` can hold only one value (0 or 1, not both), so _P_0 and _P_1 could **not** have passed their `while` conditions at the same time.
- Therefore one process — say _P__j_ — passed first, while _P__i_ still had to evaluate the extra condition `turn == j`. At that moment `flag[j] == true` and `turn == j`, and this condition persists for as long as _P__j_ is in its critical section. Hence mutual exclusion is preserved.

### Progress and bounded waiting

- A process _P__i_ can be blocked from entering only if it is stuck in the `while` loop with `flag[j] == true` and `turn == j` (the only loop possible).
- **If _P__j_ is not ready** (`flag[j] == false`) → _P__i_ enters immediately.
- **If _P__j_ is ready and also in its `while` loop**, then `turn` is either `i` or `j`:
	- `turn == i` → _P__i_ enters.
	- `turn == j` → _P__j_ enters. On exiting, _P__j_ resets `flag[j] = false`, letting _P__i_ enter. If _P__j_ tries to re-enter, it must first set `turn = i`.
- Since _P__i_ never changes `turn` while waiting, _P__i_ will enter the critical section (**progress**) after at most one entry by _P__j_ (**bounded waiting**).

## Related

- [[> Peterson's Solution]] — back to the sub-topic MOC
- [[> Process Synchronization]] — back to the chapter MOC
- [[The Critical-Section Problem]] — the three requirements this solution satisfies
