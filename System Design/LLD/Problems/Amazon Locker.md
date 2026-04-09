---
tags: [lld, problem, system, access-control]
aliases: [Locker System]
---

# Amazon Locker

A self-service package pickup system. Drivers deposit packages into compartments; customers retrieve them with a generated access code.

---

## Requirements

1. Carrier deposits a package by specifying size (SMALL / MEDIUM / LARGE)
   - System assigns a matching compartment, opens it, returns an access token
   - Error if no compartment of that size is available
2. One access token per package — 1:1 mapping
3. Customer retrieves package by entering access token
   - Specific error for invalid code vs expired code
4. Access tokens expire after 7 days — expired tokens are rejected, but package stays until staff removes it
5. Staff operation: open all compartments with expired tokens for manual package removal

**Out of scope:** delivery logistics, customer notification (SMS/email), lockout after failed attempts, UI, multiple locker stations, payment

---

## Entities & Relationships

**Package** — not an entity. Our system only needs its size, which is just an input parameter to `depositPackage(size)`. The package itself lives in a fulfillment system.

| Entity | Responsibility |
|---|---|
| `Locker` | Orchestrator. Owns all compartments and the token lookup map. Runs deposit and pickup flows. |
| `AccessToken` | Bearer token for compartment access. Holds the code, expiration, and a reference to the compartment it unlocks. Enforces its own expiry. |
| `Compartment` | Physical locker slot. Owns its size and occupancy state. |

```
Locker (orchestrator)
 ├── Compartment[]
 └── Map<tokenCode, AccessToken>
          └── Compartment (ref)
```

---

## Class Design

```
enum Size:      SMALL | MEDIUM | LARGE

class Locker:
  - compartments: Compartment[]
  - accessTokenMapping: Map<string, AccessToken>

  + Locker(compartments)
  + depositPackage(size) -> string | error     // returns token code
  + pickup(tokenCode) -> void | error          // opens compartment or throws
  + openExpiredCompartments() -> void

class AccessToken:
  - code: string
  - expiration: timestamp
  - compartment: Compartment

  + AccessToken(code, expiration, compartment)
  + isExpired() -> bool
  + getCompartment() -> Compartment
  + getCode() -> string

class Compartment:
  - size: Size
  - occupied: bool

  + Compartment(size)
  + getSize() -> Size
  + isOccupied() -> bool
  + markOccupied() -> void
  + markFree() -> void
  + open() -> void
```

> [!tip] Why `depositPackage` returns only the token code?
> The physical door opens automatically — the driver doesn't need a compartment number. The token code is what needs to reach the customer.

> [!tip] Why `pickup` returns void?
> The door opens in front of the customer. Errors carry the actionable information (invalid vs expired), not the return value.

> [!warning] Physical vs Relational state
> `occupied` lives on `Compartment` because it describes physical presence — a package is either in the slot or it isn't, regardless of token validity. Token validity and physical occupancy can diverge (expired token but package still present), so you can't derive one from the other. See [[LLD - Entities and Relationships#Where Does State Belong?]].

---

## Implementation

### `depositPackage`

```python
depositPackage(size)
    compartment = getAvailableCompartment(size)
    if compartment == null
        throw Error("No available compartment of size " + size)

    compartment.open()
    compartment.markOccupied()
    accessToken = generateAccessToken(compartment)
    accessTokenMapping[accessToken.getCode()] = accessToken

    return accessToken.getCode()

getAvailableCompartment(size)
    for compartment in compartments
        if compartment.getSize() == size && !compartment.isOccupied()
            return compartment
    return null

generateAccessToken(compartment)
    code = generateRandomCode()
    expiration = now() + 7.days()
    return AccessToken(code, expiration, compartment)
```

> [!tip] Occupancy tracking — why not derive it from the token map?
> Tempting: "a compartment is occupied if it has a token." But expired tokens stay in the map (so we can give specific "expired" errors), meaning an expired token would falsely mark the compartment as occupied. Physical occupancy and token validity are different concepts that diverge over time.

---

### `pickup`

All validation happens before any state mutation.

```
pickup(tokenCode)
    if tokenCode == null || tokenCode.isEmpty()
        throw Error("Invalid access token code")

    accessToken = accessTokenMapping[tokenCode]
    if accessToken == null
        throw Error("Invalid access token code")    // same message for used or never-existed

    if accessToken.isExpired()
        throw Error("Access token has expired")     // specific — user knows to contact support

    compartment = accessToken.getCompartment()
    compartment.open()
    clearDeposit(accessToken)

clearDeposit(accessToken)
    accessToken.getCompartment().markFree()
    accessTokenMapping.remove(accessToken.getCode())
```

> [!tip] "Invalid code" covers both wrong codes and already-used codes
> After a successful pickup, the token is removed from the map. A second attempt with the same code looks identical to a random wrong code — no need to track used codes separately. Only expired tokens get a distinct message because that's *actionable* ("contact support").

---

### `openExpiredCompartments`

```
openExpiredCompartments()
    for tokenCode, accessToken in accessTokenMapping
        if accessToken.isExpired()
            accessToken.getCompartment().open()
```

Note: we don't call `clearDeposit` here — the package is still physically present until staff removes it. Cleanup (marking free + removing token) happens via a separate staff-confirmation step (out of scope).

---

## Verification Trace

```
Initial: compartments=[A(S), B(M), C(L)], all occupied=false, tokenMap={}

depositPackage(MEDIUM)
  → getAvailableCompartment(MEDIUM) = B
  → B.open(), B.markOccupied()
  → token "ABC123" (exp=+7d, compartment=B)
  → tokenMap = {"ABC123" → token}
  → returns "ABC123"

pickup("ABC123")
  → token found, not expired
  → B.open()
  → clearDeposit: B.markFree(), tokenMap.remove("ABC123")
  → returns void, door opened

pickup("ABC123") [again]
  → tokenMap.get("ABC123") = null
  → throw "Invalid access token code"

--- 8 days later ---

pickup("ABC123") [if token still in map from another flow]
  → token.isExpired() = true
  → throw "Access token has expired"

openExpiredCompartments()
  → iterates tokenMap, finds expired tokens
  → opens corresponding compartments for staff
  → (staff physically removes packages, then triggers cleanup out of scope)
```

---

## Extensibility

**Size fallback** — if no MEDIUM available, try LARGE. Change `getAvailableCompartment` to iterate sizes from requested upward. No other class changes.

```
getAvailableCompartment(requestedSize)
    sizesInOrder = [SMALL, MEDIUM, LARGE]
    for size in sizesInOrder[requestedSize.index..]
        for c in compartments
            if c.getSize() == size && !c.isOccupied()
                return c
    return null
```

**Broken/maintenance compartments** — replace the `occupied: bool` with a `status` enum. `getAvailableCompartment` checks `isAvailable()` which returns true only for `AVAILABLE` status.

```
enum CompartmentStatus: AVAILABLE | OCCUPIED | OUT_OF_SERVICE

isAvailable()
    return status == AVAILABLE
```

**Two-phase deposit (verify package is actually placed)** — split `depositPackage` into `reserveCompartment(size) -> reservationId` and `confirmDeposit(reservationId) -> tokenCode`. Access token is only generated after the driver confirms. Add `RESERVED` to `CompartmentStatus`. Requires timeout logic to auto-cancel stale reservations.

> [!warning] Two-phase tradeoff
> Adds a new state, a reservation map, and timeout management. Worth it in production (physical sensors or manual confirm). Overkill for interview scope.

---

## Related
- [[> LLD Delivery Framework]]
- [[LLD - Entities and Relationships]]
- [[LLD - Class Design]]
- [[LLD - Implementation]]
- [[Connect Four]]
