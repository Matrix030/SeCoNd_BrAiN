---
tags: [lld, problem, game, state-machine]
aliases: [Connect 4]
---

# Connect Four

Two players drop discs into a 7×6 grid. First to align four in a row, column, or diagonal wins.

---

## Requirements

1. Players alternate dropping discs into columns 0–6; disc falls to the lowest empty row
2. Game ends when a player connects four (horizontal, vertical, or diagonal) → **WIN**, or the board fills → **DRAW**
3. Reject invalid moves: full column, wrong player's turn, move after game over

**Out of scope:** UI, concurrent games, move history, undo, configurable board size

---

## Entities & Relationships

```
Game (orchestrator)
 ├── Board       (owns grid state + win detection)
 └── Player ×2  (data only — name + disc color)
```

---

## Class Design

```
enum GameState:   IN_PROGRESS | WON | DRAW
enum DiscColor:   RED | YELLOW

class Game:
  - board: Board
  - player1, player2: Player
  - currentPlayer: Player
  - state: GameState
  - winner: Player?           // null if no winner yet or draw

  + makeMove(player, column) -> bool
  + getCurrentPlayer() -> Player
  + getGameState() -> GameState
  + getWinner() -> Player?
  + getBoard() -> Board

class Board:
  - rows: int = 6
  - cols: int = 7
  - grid: DiscColor?[rows][cols]

  + canPlace(column) -> bool
  + placeDisc(column, color) -> int   // returns row where disc lands, -1 if invalid
  + isFull() -> bool
  + checkWin(row, column, color) -> bool
  + getCell(row, column) -> DiscColor?

class Player:
  - name: string
  - color: DiscColor

  + getName() -> string
  + getColor() -> DiscColor
```

> [!tip] Why DiscColor in the grid (not Player)?
> Storing `DiscColor` keeps `Board` independently testable — no need to mock a `Player` object just to check grid state.

> [!warning] GameState — enum not boolean flags
> Three booleans (`isOver`, `hasWinner`, `isDraw`) allow 8 combinations but only 3 are valid. An enum makes invalid states impossible. See [[LLD - Class Design#Make Invalid States Unrepresentable]].

---

## Implementation

### `makeMove`

All validation happens before any state mutation.

```
makeMove(player, column)
    if state != IN_PROGRESS → return false
    if player != currentPlayer → return false

    row = board.placeDisc(column, player.getColor())
    if row == -1 → return false          // board handles column/bounds validation

    if board.checkWin(row, column, player.getColor())
        state = WON
        winner = player
    else if board.isFull()
        state = DRAW
    else
        currentPlayer = (player == player1) ? player2 : player1

    return true
```

> [!tip] Why pass `player` to `makeMove`?
> Lets callers (e.g. a networked client) tag moves with identity. Alternative: use `currentPlayer` implicitly — simpler if only your own code calls it. Either is fine; explain the choice.

---

### `placeDisc`

Scan bottom-up for the first empty cell.

```
placeDisc(column, color)
    if column < 0 || column >= cols → return -1
    if !canPlace(column) → return -1

    for row = rows-1 downto 0:
        if grid[row][column] == null
            grid[row][column] = color
            return row
    return -1
```

`canPlace` delegates to checking the top row:
```
canPlace(column)
    if column < 0 || column >= cols → return false
    return grid[0][column] == null
```

---

### `checkWin`

Four directions, count contiguous discs in both halves.

```
checkWin(row, col, color)
    if out of bounds || grid[row][col] != color → return false

    directions = [(0,1), (1,0), (1,1), (-1,1)]   // →, ↓, ↘, ↗
    for (dr, dc) in directions:
        count = 1
            + countInDir(row, col, dr,  dc,  color)
            + countInDir(row, col, -dr, -dc, color)
        if count >= 4 → return true
    return false

countInDir(row, col, dr, dc, color)
    count = 0
    r, c = row+dr, col+dc
    while inBounds(r,c) && grid[r][c] == color
        count++; r += dr; c += dc
    return count
```

> [!tip] Direction vectors over 4 separate methods
> One `countInDir` helper handles all four axes. Adding a new direction is one line.

---

## Verification Trace

```
Board (partial): row5=[R,Y,R,_,_,_,_], row4=[R,Y,_,_,_,_,_]
currentPlayer = player1 (RED)

makeMove(p1, col=0) → placeDisc → row=3, checkWin(3,0,RED)
  vertical: (4,0)=R, (5,0)=R → count=3. No win. Switch to p2.

makeMove(p2, col=1) → row=3, no win. Switch to p1.

makeMove(p1, col=0) → row=2, checkWin(2,0,RED)
  vertical down: (3,0)=R,(4,0)=R,(5,0)=R → count=4 ✓
  state=WON, winner=player1

makeMove(p2, col=1) → state != IN_PROGRESS → false ✓
```

---

## Extensibility

**Configurable board size** — `rows`/`cols` are already variables on `Board`. Make them constructor params; all placement and win logic already uses them dynamically.

**Undo** — All state mutations flow through `makeMove`. Add a `moveHistory: Stack<Move>` to `Game`. On `undo()`: pop last move, call `board.clearCell(row, col)`, revert `currentPlayer`, reset `state = IN_PROGRESS`.

**Bot opponent** — Add a `BotEngine` with `chooseMove(game) -> int` that picks a column. `Game` and `Board` don't change — from their perspective a bot move is just another `makeMove(currentPlayer, column)` call.

> [!tip] Bot as BotEngine, not a Player subclass
> Player is just data (name + color). Making it an interface for `HumanPlayer`/`BotPlayer` adds abstraction without value. Keep identity and decision-making separate.

---

## Related
- [[> LLD Delivery Framework]]
- [[LLD - Requirements]]
- [[LLD - Class Design]]
- [[LLD - Implementation]]
- [[LLD - Extensibility]]
