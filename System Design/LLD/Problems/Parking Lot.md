---
tags: [lld, problem, design-patterns, oop]
aliases: [Parking Lot System, Parking Lot]
---

# Parking Lot

Manages vehicle spot assignment across a parking lot. Vehicles enter and receive a ticket; on exit the system validates the ticket, calculates the fee, and frees the spot.

---

## Requirements

1. Three vehicle types: `Motorcycle`, `Car`, `Large`
2. On entry, system auto-assigns a compatible available spot and issues a ticket
3. On exit, user provides ticket ID → system validates, calculates hourly fee (rounded up), and frees the spot
4. Pricing is hourly, same rate for all vehicle types
5. Reject entry if no compatible spot is available
6. Reject exit if ticket is invalid or already used

**Out of scope:** payment processing, physical gate hardware, security cameras, UI, reservations

---

## Entities & Relationships

**Vehicle** — not an entity. External to the system. Only its type matters for spot matching → just an enum.

| Entity | Responsibility |
|---|---|
| `ParkingLot` | Orchestrator. Owns all spots and active tickets. Assigns spots at entry, validates and prices at exit. The only public API surface. |
| `ParkingSpot` | Pure data holder for a physical space: ID and spot type. Does not track its own occupancy. |
| `Ticket` | Immutable record of a parking session: ticket ID, spot ID, vehicle type, entry timestamp. No business logic. |

```
ParkingLot
 ├── spots: List<ParkingSpot>
 ├── occupiedSpotIds: Set<String>   ← maintained index (not derived on demand)
 ├── activeTickets: Map<String, Ticket>
 └── hourlyRateCents: long
```

> [!tip] Where does occupancy live?
> Three options: (1) flag on `ParkingSpot`, (2) derive from `activeTickets` on demand, (3) maintained `Set` in `ParkingLot`. We use the `Set` — it's a clean concurrency boundary and makes "available?" O(1) without scanning tickets. The trade-off is a second source of truth that must stay in sync with `activeTickets`.

> [!note] Intrinsic vs relational state
> Spot ID and type never change → intrinsic, belongs on `ParkingSpot`. Occupancy is relational ("a ticket is currently assigned to this spot") → belongs in the orchestrator. This is why `ParkingSpot` has no `occupied` flag.

---

## Class Design

```
class ParkingLot:
    - spots: List<ParkingSpot>
    - occupiedSpotIds: Set<String>
    - activeTickets: Map<String, Ticket>
    - hourlyRateCents: long

    + ParkingLot(spots, hourlyRateCents)
    + enter(vehicleType) -> Ticket
    + exit(ticketId) -> long              // returns fee in cents


class ParkingSpot:
    - id: String
    - spotType: SpotType

    + ParkingSpot(id, spotType)
    + getId() -> String
    + getSpotType() -> SpotType


class Ticket:
    - id: String
    - spotId: String
    - vehicleType: VehicleType
    - entryTime: long

    + Ticket(id, spotId, vehicleType, entryTime)
    + getId() -> String
    + getSpotId() -> String
    + getVehicleType() -> VehicleType
    + getEntryTime() -> long


enum SpotType:   MOTORCYCLE | CAR | LARGE
enum VehicleType: MOTORCYCLE | CAR | LARGE
```

> [!tip] Two separate enums, same values
> `SpotType` and `VehicleType` are separate types even though they mirror each other. If a future requirement says "motorcycles can use car spots when motorcycle spots are full," having distinct enums makes that condition expressible without ambiguity.

> [!warning] Fee calculation belongs in ParkingLot, not Ticket
> Putting `calculateFee()` on `Ticket` would require storing the hourly rate there and gives the ticket two reasons to change. Pricing is a policy enforced by the lot — it stays in the orchestrator. `Ticket` is just a receipt.

> [!warning] Store money as cents (long), not floats
> Floating-point types can't represent decimal fractions exactly. Store the smallest unit (cents) as an integer. `$5.47 → 547`. All arithmetic stays exact; convert to dollars only for display.

---

## Implementation

### `enter`

```
enter(vehicleType)
    spot = findAvailableSpot(vehicleType)
    if spot == null
        throw Error("No available spots")

    occupiedSpotIds.add(spot.id)

    ticket = new Ticket(generateId(), spot.id, vehicleType, currentTime())
    activeTickets[ticket.id] = ticket

    return ticket
```

State changes only happen *after* a spot is found. If no spot exists, nothing is modified.

---

### `exit`

```
exit(ticketId)
    if ticketId == null
        throw Error("Invalid ticket ID")

    ticket = activeTickets[ticketId]
    if ticket == null
        throw Error("Ticket not found or already used")

    fee = computeFee(ticket.entryTime, currentTime())

    occupiedSpotIds.remove(ticket.spotId)   // free the spot
    activeTickets.remove(ticketId)           // prevent double exit

    return fee
```

Removing the ticket from `activeTickets` prevents re-use. We don't distinguish "never existed" from "already used" — both return the same error.

---

### `findAvailableSpot`

```
findAvailableSpot(vehicleType)
    requiredType = mapVehicleTypeToSpotType(vehicleType)
    for spot in spots
        if spot.spotType == requiredType and spot.id not in occupiedSpotIds
            return spot
    return null
```

Linear scan. Each iteration is O(1) thanks to the `Set` lookup. No cleverness — first match wins.

---

### `computeFee`

```
computeFee(entryTime, exitTime)
    durationMs = exitTime - entryTime
    msPerHour = 1000 * 60 * 60

    durationHours = durationMs / msPerHour
    if durationMs % msPerHour > 0
        durationHours++               // any partial hour rounds up

    return durationHours * hourlyRateCents
```

5 minutes parked → charged for 1 hour. No separate "minimum charge" logic needed — rounding up handles it.

---

## Verification Trace

```
Setup: spots=[A(MOTO), B(CAR), C(LARGE)], rate=500¢/hr
       occupiedSpotIds={}, activeTickets={}

enter(CAR)
  → findAvailableSpot(CAR) → spot B (type matches, not occupied)
  → occupiedSpotIds = {"B"}
  → ticket T1 = {id="T1", spotId="B", vehicleType=CAR, entryTime=0}
  → activeTickets = {"T1" → T1}
  → return T1 ✓

exit("T1") called 2.5 hours later (entryTime=0, exitTime=9,000,000ms)
  → ticket found
  → durationMs=9,000,000, durationHours=2, remainder>0 → durationHours=3
  → fee = 3 * 500 = 1500¢ ✓
  → occupiedSpotIds = {}, activeTickets = {}

exit("T1") again
  → activeTickets["T1"] == null → Error("Ticket not found or already used") ✓

enter(CAR) when all CAR spots occupied
  → findAvailableSpot(CAR) returns null → Error("No available spots") ✓
```

---

## Extensibility

**Multi-floor garage** — introduce `ParkingFloor` between `ParkingLot` and spots. Each floor owns its spot list. `findAvailableSpot` iterates floors; spot IDs encode floor info (e.g. `"3-A15"`). `Ticket` is unchanged.

```
class ParkingFloor:
    - floorNumber: int
    - spots: List<ParkingSpot>

    + findAvailableSpot(spotType) -> ParkingSpot
    + getAvailableSpotCount(spotType) -> int
```

Allocation strategy becomes a choice: fill lower floors first (simplest), balance load across floors, or proximity to destination.

**Per-vehicle-type pricing** — replace `hourlyRateCents: long` with `hourlyRates: Map<VehicleType, long>`. `computeFee` takes `vehicleType` from the ticket and looks up the rate. No other changes.

**Concurrent access (multiple entrances)** — the gap between `findAvailableSpot` and `occupiedSpotIds.add()` is a race condition: two threads can find the same spot available simultaneously. Fix with per-method synchronization for correctness, or atomic check-and-add on the `Set` with retry for throughput. For a typical lot with 3–5 entrances, method-level locking is sufficient.

> [!warning] Race condition window
> Thread A finds spot B free. Before it adds "B" to `occupiedSpotIds`, Thread B also finds spot B free. Both issue tickets for B. Fix: the find-and-mark operation must be atomic.

---

## Related
- [[> LLD Delivery Framework]]
- [[> LLD Design Patterns]]
- [[LLD - Class Design]]
- [[Concurrency - Scarcity]]
- [[Amazon Locker]]
