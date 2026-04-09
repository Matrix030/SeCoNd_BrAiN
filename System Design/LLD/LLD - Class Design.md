---
tags: [lld, interview, class-design, encapsulation]
aliases: [LLD Class Design Phase]
---

# LLD - Class Design

Turn each entity into a class outline with explicit state and behavior. (~10-15 minutes)

---

## Process

Work **top-down** — start with the orchestrator, then supporting entities. For each class, answer:

1. **State** — What does this class need to *remember*?
2. **Behavior** — What does this class need to *do*?

> [!tip] Anchor to requirements
> Derive both state and behavior directly from your requirements list. This avoids guessing and bloat.

## Deriving State

For each entity, ask: *Which requirements does this entity own? What must it track?*

| Requirement | What Game must track |
|---|---|
| Two players alternate moves | The two players, whose turn it is, the Board |
| Game ends on win or full board | Game state (IN_PROGRESS, WON, DRAW), winner |

## Deriving Behavior

Ask: *What operations does the outside world need from this class?*

| Need | Method |
|---|---|
| Players make moves | `makeMove(player, row, col) -> bool` |
| Check whose turn | `getCurrentPlayer() -> Player` |
| Check game state | `getGameState() -> GameState` |
| See who won | `getWinner() -> Player?` |

## Make Invalid States Unrepresentable

When modeling state, the type structure should match the domain — so impossible states can't exist.

**Bad — boolean flags:**
Three booleans (`isOver`, `hasWinner`, `isDraw`) allow 8 combinations, but the domain only has 3 valid states. Nothing stops `isOver=false, hasWinner=true`.

**Good — enum:**
```
enum GameState: IN_PROGRESS | WON | DRAW
```
One field, three values, no invalid combinations. When you add a new state, you add one enum value — not a new flag and a pile of synchronization logic.

> [!tip] The ideal (in languages that support it)
> Rust, Swift, Kotlin (sealed classes), and TypeScript (discriminated unions) let you embed data inside enum variants: `WON(winner: Player)`. If the state is `WON`, a winner is guaranteed. If it's `DRAW`, there's no winner field to corrupt.
> Java/Python don't support this elegantly — a nullable `winner` field alongside the enum is the right call there. Just be aware you're trusting yourself to keep them in sync.

---

## Encapsulation Principle

> [!warning] Tell, Don't Ask
> Objects should manage their own state and expose behavior, not getters for callers to make decisions.

- **Workflow/lifecycle rules** → orchestrator (e.g. Game)
- **Data-specific rules** → entity that owns the data (e.g. Board checks if a cell is occupied)

## Example Output

```
class Game:
  - board: Board
  - playerX: Player
  - playerO: Player
  - currentPlayer: Player
  - state: GameState (IN_PROGRESS, WON, DRAW)
  - winner: Player?

  + makeMove(player, row, col) -> bool
  + getCurrentPlayer() -> Player
  + getGameState() -> GameState
  + getWinner() -> Player?
  + getBoard() -> Board
```

## On UML

Skip it. It's outdated — engineers design in code. Simple class notation (as above) is faster and clearer. If an interviewer insists, ask if simplified notation is acceptable — it usually is.

---

## Related
- [[LLD - Entities and Relationships]]
- [[LLD - Implementation]]
- [[> LLD Delivery Framework]]
