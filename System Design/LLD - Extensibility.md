---
tags: [lld, interview, extensibility]
aliases: [LLD Extensibility Phase]
---

# LLD - Extensibility

Show your design can evolve cleanly under follow-up questions. (~5 min, if time allows)

---

## What to Expect by Level

| Level | Expectation |
|---|---|
| Junior | Little or no extensibility discussion |
| Mid-level | 1-2 small follow-ups |
| Senior | Several "what if we..." questions in a row |

## How to Handle Extensions

The pattern is always the same:
1. Interviewer proposes a change
2. You **point to where** in your design it's handled
3. Explain the change **stays contained** — no major restructuring

> [!tip] Stay high level
> You're not rewriting code. You're pointing to the parts of your design that make the change clean.

## Example — Adding Undo

> *"How would you add undo functionality?"*

"All state transitions flow through `makeMove`. I'd introduce a **command history stack** — each successful action records previous state before modifying. An `undo()` method pops the stack and reverts. The rest of the system doesn't change."

This works because state mutations are **isolated** in a single method.

## The Goal

Demonstrate that your initial design is **extensible and robust** — it handles natural follow-ups without falling apart or turning into special cases.

---

## Related
- [[LLD - Implementation]]
- [[> LLD Delivery Framework]]
