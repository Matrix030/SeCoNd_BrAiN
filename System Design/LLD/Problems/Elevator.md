---
tags: [lld, problem, simulation, algorithm, state-machine]
aliases: [Elevator System, Elevator Control System]
---

# Elevator

A system managing multiple elevators serving building floors — handles hall calls, destination requests, and efficient dispatch.

---

## Requirements

1. 3 elevators serving 10 floors (0–9), fixed configuration
2. Hall calls from floors specify direction (UP or DOWN) — system picks which elevator to dispatch
3. Inside the elevator, passengers select one or more destination floors (no direction)
4. Simulation runs in discrete time steps via `step()`
5. Two stop types:
   - **Hall call**: floor + direction (PICKUP_UP / PICKUP_DOWN)
   - **Destination**: floor only (no direction — stop regardless)
6. Invalid floor numbers are rejected. Request for current floor = no-op.

**Out of scope:** weight limits, door mechanics, emergency stops, dynamic configuration, UI

> [!tip] Always ask: simulation or hardware control?
> Real elevator control uses motor controllers, floor sensors, and hardware interrupts — no `step()`. Interviews almost always want the simulation model. Ask explicitly: *"Are we building a simulation where I control time, or modeling actual control software?"* It signals senior-level awareness.

---

## Entities & Relationships

**Floor** — not an entity. Just an integer. Floors don't have state or behavior.

**Request** — worth its own class. A stop isn't just a floor number — it has a type (pickup going up, pickup going down, or destination). The type determines whether the elevator stops there on a given pass. Without it, the elevator can't distinguish "stop for someone going down" from "stop for a destination button."

| Entity | Responsibility |
|---|---|
| `ElevatorController` | Orchestrator. Receives hall calls, dispatches to the right elevator, advances all elevators each tick. |
| `Elevator` | One car. Owns position, direction, and its request queue. Executes movement logic. |
| `Request` | A stop: floor + type. Enables direction-aware stopping. |

```
ElevatorController
 └── Elevator ×3
       └── Set<Request>
```

---

## Class Design

```
enum Direction:     UP | DOWN | IDLE
enum RequestType:   PICKUP_UP | PICKUP_DOWN | DESTINATION

class ElevatorController:
  - elevators: List<Elevator>

  + ElevatorController()
  + requestElevator(floor, type: RequestType) -> bool   // hall calls only
  + step() -> void

class Elevator:
  - currentFloor: int
  - direction: Direction
  - requests: Set<Request>

  + Elevator()
  + addRequest(request) -> bool
  + step() -> void
  + getCurrentFloor() -> int
  + getDirection() -> Direction
  + hasRequestsAtOrBeyond(floor, dir) -> bool

class Request:
  - floor: int
  - type: RequestType

  + Request(floor, type)
  + getFloor() -> int
  + getType() -> RequestType
  // equals + hashCode on (floor, type)
```

> [!tip] RequestType, not Direction for hall calls
> Hall calls are never IDLE — using `Direction` would allow an invalid value. `RequestType` (PICKUP_UP / PICKUP_DOWN / DESTINATION) makes that impossible at the type level.

> [!tip] Why IDLE on Direction?
> Without IDLE, there's no way to represent "not moving." Elevators with an empty queue would have no valid state.

> [!warning] Don't extract a shared interface between `requestElevator` and `addRequest`
> They look similar but do fundamentally different things — one is system coordination, the other is state mutation on a single car. Just because two methods share similar parameters doesn't mean they need a common interface.

---

## Implementation

### Dispatch — `selectBestElevator`

Three strategies, in order of sophistication:

**1. Nearest (simplest)**
Pick the elevator with the smallest distance to the requested floor.

```
selectBestElevator(request)
    return elevator with min |e.getCurrentFloor() - request.getFloor()|
```

Problem: sends an elevator that's heading the wrong way. It'll pass the floor without stopping, reverse, then come back. Correct but poor experience.

**2. Direction-aware**
Prefer elevators already moving toward the floor in the right direction.

```
Priority 1 → elevator moving toward floor in matching direction
Priority 2 → idle elevator (nearest)
Priority 3 → any elevator (nearest)
```

Problem: doesn't check if the elevator's existing requests will actually take it to or past the floor. An elevator at floor 3 going UP with only a stop at floor 4 would be dispatched for a floor 7 UP call — but it'll reverse at 4 and never get there.

**3. Request-aware (best)**
Same as direction-aware, but add a check: does the elevator have stops that will carry it to or past the requested floor?

```
findCommittedToFloor(request)
    for e in elevators:
        if direction doesn't match → skip
        if not positioned before the floor → skip
        if !e.hasRequestsAtOrBeyond(floor, direction) → skip   // NEW
        track nearest
```

```
hasRequestsAtOrBeyond(floor, dir)
    for req in requests:
        if dir == UP  && req.floor >= floor && req.type in {PICKUP_UP, DESTINATION}  → true
        if dir == DOWN && req.floor <= floor && req.type in {PICKUP_DOWN, DESTINATION} → true
    return false
```

> [!tip] What to implement in the interview
> Code the direction-aware version, then proactively say: *"This doesn't verify the elevator's queue actually extends past the requested floor — I'd add `hasRequestsAtOrBeyond` to handle that properly."* Shows awareness without burning time.

> [!tip] Strategy pattern opportunity
> If the problem needs swappable scheduling algorithms (different buildings, different tradeoffs), `selectBestElevator` becomes an interface. Each algorithm implements it. [[> LLD Design Patterns#Strategy]]

---

### Movement — `Elevator.step()` (SCAN algorithm)

Three movement algorithms:

| Algorithm | Behavior | Problem |
|---|---|---|
| FIFO | Service requests in arrival order | Constant direction changes, terrible experience |
| Nearest-first | Always go to closest floor | Still reverses unnecessarily, feels unpredictable |
| **SCAN** | Sweep in one direction, service all stops, reverse when clear | Predictable, efficient, matches passenger intuition |

**SCAN implementation:**

```
step()
    // Case 1: empty — go idle
    if requests.isEmpty()
        direction = IDLE; return

    // Case 2: idle but has requests — pick direction toward nearest
    if direction == IDLE
        nearest = min(requests, by distance then by floor)   // deterministic!
        direction = (nearest.floor > currentFloor) ? UP : DOWN

    // Case 3: stop at current floor if there's a matching request
    pickupReq = Request(currentFloor, direction==UP ? PICKUP_UP : PICKUP_DOWN)
    destReq   = Request(currentFloor, DESTINATION)
    if requests.contains(pickupReq) || requests.contains(destReq)
        requests.remove(pickupReq)
        requests.remove(destReq)
        if requests.isEmpty() → direction = IDLE
        return   // stopped this tick, don't move

    // Case 4: no requests ahead — reverse
    if !hasRequestsAhead(direction)
        direction = (direction==UP) ? DOWN : UP
        return   // don't move this tick, re-check stops next tick

    // Case 5: move one floor
    currentFloor += (direction == UP) ? 1 : -1

hasRequestsAhead(dir)
    return any request where:
        dir==UP  && req.floor > currentFloor, or
        dir==DOWN && req.floor < currentFloor
```

> [!warning] Stopping ≠ moving in the same tick
> Cases 3 and 4 both `return` early. The elevator either stops OR reverses OR moves — never two of these in one tick.

> [!warning] Never use iterator().next() on a HashSet for direction choice
> HashSet iteration order is non-deterministic. Pick the nearest request by a consistent rule (e.g. smallest distance, then lowest floor). Otherwise your simulation produces different results across runs.

> [!tip] Floor boundaries fall out naturally
> No need for `if currentFloor == 9 then reverse`. `hasRequestsAhead(UP)` returns false at floor 9 if there are no requests above — reversal happens automatically. Adding explicit boundary checks creates bugs (forces reversal even when a stop exists at the boundary floor).

> [!tip] Stopping is direction-aware, traveling is not
> The elevator only stops for matching direction requests. But `hasRequestsAhead` checks for ANY request in that direction — the elevator must travel toward all requests, even ones it won't stop for until the return trip.

---

## Verification Trace

```
Elevator at floor 3, direction=UP, requests={Request(5,PICKUP_UP), Request(7,DESTINATION)}

Tick 0: not at a stop → move to 4
Tick 1: not at a stop → move to 5
Tick 2: at floor 5, found Request(5,PICKUP_UP) → remove, still has Request(7), stay UP. Don't move.
Tick 3: move to 6
Tick 4: move to 7
Tick 5: at floor 7, found Request(7,DESTINATION) → remove, requests empty → IDLE. Don't move.

New hall call: requestElevator(2, PICKUP_DOWN) → adds Request(2,PICKUP_DOWN)

Tick 6: direction=IDLE, nearest=floor 2 → direction=DOWN. hasRequestsAhead(DOWN)? Yes → move to 6
...
Tick 11: arrive at floor 2, found Request(2,PICKUP_DOWN) → remove → IDLE
```

---

## Extensibility

**Express elevator** — add `isExpress: bool` and `expressFloors: Set<int>` to `Elevator`. `addRequest` rejects non-express floors. In the controller's dispatch, prefer the express elevator for express-floor calls when it's idle.

**Cancel a request** — add `removeRequest(request)` to `Elevator`. Removes from the set. If the elevator has already passed the floor, the request is already gone — no-op. Movement logic in `step()` is unchanged.

**Concurrency (concurrent hall calls)** — two options:
- **Lock**: single lock around both `requestElevator` and `step()`. Simple, correct, slight contention.
- **Pending queue**: `addRequest` writes to a thread-safe queue; `step()` drains it into the working set at the start of each tick. No lock needed, zero contention between callers and the simulation loop.

---

## Related
- [[> LLD Delivery Framework]]
- [[LLD - Class Design]]
- [[LLD - Implementation]]
- [[> LLD Design Patterns#Strategy]]
- [[Connect Four]]
- [[Amazon Locker]]
