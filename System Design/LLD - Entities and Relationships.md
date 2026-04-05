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

> [!warning] Don't over-model
> Not every noun deserves a class. Keep it lean — too many micro-objects bloats the design.

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
