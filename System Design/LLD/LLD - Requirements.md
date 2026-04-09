---
tags: [lld, interview, requirements]
aliases: [LLD Requirements Phase]
---

# LLD - Requirements

Turn a minimal prompt into a spec you can actually design around. (~5 minutes)

---

## The Problem

Interview prompts are intentionally vague — e.g. *"Design Tic Tac Toe"*. Your job is to make it unambiguous by asking focused questions.

## Question Themes

Use these four themes to generate questions for **any** domain:

1. **Primary capabilities** — What operations must the system support?
2. **Rules and completion** — What defines success, failure, or state transitions?
3. **Error handling** — How should invalid inputs/actions be handled?
4. **Scope boundaries** — What's in scope (core logic, business rules) vs. explicitly out (UI, storage, networking, concurrency)?

> [!tip] Write it down
> Confirm your spec with the interviewer on the shared whiteboard. Include an **Out of Scope** section to prevent scope creep.

## Example — Tic Tac Toe

**Requirements:**
1. Two players alternate placing X and O on a 3x3 grid.
2. A player wins by completing a row, column, or diagonal.
3. Draw if all nine cells are filled with no winner.
4. Invalid moves are rejected (occupied cell, move after game over).
5. System provides a way to query game state and reset.

**Out of Scope:**
- UI/rendering, AI opponent, networked multiplayer, variable board sizes, undo/redo.

---

## Related
- [[> LLD Delivery Framework]]
- [[LLD - Entities and Relationships]]
