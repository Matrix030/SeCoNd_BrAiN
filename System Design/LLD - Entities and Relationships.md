---
tags: [lld, interview, entities, relationships]
aliases: [LLD Entities Phase]
---

# LLD - Entities and Relationships

Translate requirements into core entities with clean ownership boundaries. (~3 minutes)

---

## Identify Entities

Scan your requirements and pull out meaningful **nouns**. Apply this filter:

- **Maintains changing state or enforces rules?** → Its own entity.
- **Just information attached to something else?** → A field on another class.
- **Only appears as an input to an operation?** → A parameter, not an entity.

> [!warning] Don't over-model
> Not every noun deserves a class. Keep it lean — too many micro-objects bloats the design.

**Example:** In an Amazon Locker system, *Package* looks like an obvious entity — but the system only cares about its size. It's just an input parameter to `depositPackage(size)`. The package itself lives in another system.

## Where Does State Belong?

When deciding where to put state, ask whether it's **physical** or **relational**:

- **Physical state** (is this compartment occupied? is this door broken?) describes the entity's own condition → lives **on the entity**
- **Relational state** (which compartment is assigned to which token?) describes system-managed relationships → lives **in the orchestrator**

> [!tip] Both are defensible
> Putting `occupied` on `Compartment` and putting it in a `Set` on the orchestrator are both valid. What matters is having a rationale you can explain.

## Define Relationships

Once you have entities, think through how they interact:

- Which entity is the **orchestrator** (drives the main workflow)?
- Which entities **own durable state**?
- How do they **depend** on each other? (has-a, uses, contains)
- Where should specific **rules** logically live?

## Whiteboard Representation

Keep it simple — boxes, arrows, and labels. No formal UML needed.

```
Entities:
- Game (orchestrator)
- Board
- Player

Relationships:
- Game -> Board
- Game -> Player (2x)
```

> [!tip] Communicate, don't notate
> The whiteboard is for your interviewer to follow your thinking. Don't fixate on notation — clarity over formality.

---

## Related
- [[LLD - Requirements]]
- [[LLD - Class Design]]
- [[> LLD Delivery Framework]]
