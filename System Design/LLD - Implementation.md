---
tags: [lld, interview, implementation, pseudo-code]
aliases: [LLD Implementation Phase]
---

# LLD - Implementation

Implement the major methods of your designed classes. (~10 minutes)

---

## Before You Start

**Ask your interviewer** what level of detail they expect:
- Pseudo-code for key methods (most common)
- Near-complete code in a specific language
- Verbal walkthrough of the logic

> [!tip] Use your strongest language
> Don't pick an unfamiliar language to impress — it backfires more than it helps.

## Approach

### 1. Happy Path First

Walk through the normal flow linearly:
- What inputs it receives
- The sequence of steps
- Internal calls to other classes
- What it returns / how state changes

### 2. Edge Cases Second

Enumerate failure modes: invalid inputs, illegal operations, out-of-range values, state violations.

> [!warning] Think like production
> Handling edge cases signals you write real code, not toy logic.

### 3. Write Pseudo-code

```
makeMove(player, row, col)
    if state != IN_PROGRESS
        return false
    if player != currentPlayer
        return false
    if !board.canPlace(row, col)
        return false

    board.placeMark(row, col, player.mark)

    if board.checkWin(row, col, player.mark)
        state = WON
        winner = player
    else if board.isFull()
        state = DRAW
    else
        currentPlayer = (player == playerX) ? playerO : playerX

    return true
```

## Verification — Trace a Scenario

Spend 1-2 minutes tracing a concrete example through your code:

```
Initial: board empty, currentPlayer = X
makeMove(X, 0, 0) → board[0][0] = X, switch to O
makeMove(O, 1, 1) → board[1][1] = O, switch to X
...
```

This catches:
- Forgot to switch turns
- Win detection not triggering
- Wrong state transition order
- Edge cases breaking the flow

> [!tip] Fix on the spot
> Finding and fixing a bug during your own walkthrough is a **positive signal** — much better than the interviewer catching it.

## On Design Patterns

Patterns (Singleton, Factory, Builder, etc.) *can* add value, but candidates more often **overengineer** by forcing them where they don't belong. Only introduce a pattern if it genuinely simplifies the design.

---

## Related
- [[LLD - Class Design]]
- [[LLD - Extensibility]]
- [[> LLD Delivery Framework]]
