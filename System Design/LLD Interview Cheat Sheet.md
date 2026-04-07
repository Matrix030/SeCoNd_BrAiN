---
tags: [lld, interview, cheat-sheet, reference]
aliases: [Cheat Sheet, LLD Quick Reference]
---

# LLD Interview Cheat Sheet

Everything you need before walking into a low-level design interview.

---

## The Framework (~35 min total)

| Phase                    | Time       | What to do                                             |
| ------------------------ | ---------- | ------------------------------------------------------ |
| Requirements             | ~5 min     | Ask questions, write spec + out of scope on whiteboard |
| Entities & Relationships | ~3 min     | Identify objects, sketch ownership                     |
| Class Design             | ~10-15 min | Define state + behavior per class, top-down            |
| Implementation           | ~10 min    | Pseudo-code key methods, trace a scenario              |
| Extensibility            | ~5 min     | Point to where changes land, stay high level           |

> [!tip] If the interviewer pulls you off-framework, follow their lead — then guide back.

---

## Phase 1 — Requirements

Four question themes that work for any domain:

1. **Primary capabilities** — What must the system support?
2. **Rules and completion** — What defines success, failure, state transitions?
3. **Error handling** — What happens on invalid inputs or actions?
4. **Scope boundaries** — What's explicitly out? (UI, storage, networking, concurrency)

Always write a final spec and an **Out of Scope** list on the whiteboard.

---

## Phase 2 — Entities & Relationships

Filter every noun through:
- Maintains changing state or enforces rules? → **Entity**
- Just info attached to something else? → **Field on another class**
- Only appears as an input? → **Parameter, not an entity**

Where does state belong?
- **Physical state** (is this compartment broken?) → on the entity
- **Relational state** (assigned to which token?) → in the orchestrator

Whiteboard representation: boxes, arrows, labels. No UML needed.

---

## Phase 3 — Class Design

Work **top-down**: orchestrator first, then supporting entities.

For each class ask:
1. **State** — What must it remember to enforce its requirements?
2. **Behavior** — What does the outside world need from it?

Map requirements → state and requirements → methods directly. Avoid guessing.

**Make invalid states unrepresentable:**
- Bad: three booleans (`isOver`, `hasWinner`, `isDraw`) — allows 8 combinations, only 3 are valid
- Good: one enum (`IN_PROGRESS | WON | DRAW`) — impossible states can't exist

**Tell, Don't Ask:**
- Workflow/lifecycle rules → orchestrator
- Data-specific rules → the entity that owns the data

Skip UML. Simple class notation is faster and clearer.

---

## Phase 4 — Implementation

Ask your interviewer what they want: pseudo-code (most common), real code, or verbal walkthrough.

Order:
1. **Happy path** — normal flow, linear
2. **Edge cases** — invalid inputs, illegal state, boundary conditions
3. **Verify** — trace a concrete scenario, catch logical bugs before the interviewer does

> [!tip] Finding a bug during your own trace is a positive signal. Fix it on the spot.

Only introduce design patterns if they genuinely simplify the design. Forcing them signals over-engineering.

---

## Phase 5 — Extensibility

Pattern: interviewer proposes a change → you **point to where** it lands → explain it stays contained.

You're not rewriting code. You're showing your design has clean boundaries.

| Level | What's expected |
|---|---|
| Junior | Little or none |
| Mid-level | 1-2 follow-ups, explain the change |
| Senior | Several "what if we..." in a row, discuss tradeoffs |

---

## Design Principles

> Apply the reasoning — don't recite acronyms.

**General:**

| Principle | Rule |
|---|---|
| KISS | Simplest solution that works. Add complexity only when simplicity stops working. |
| DRY | One place per piece of logic. Don't force it if code is similar but serves different purposes. |
| YAGNI | Build for today. Talk about extensions, don't build them speculatively. |
| Separation of Concerns | Each class owns one slice. Changes stay isolated. |
| Law of Demeter | Don't reach through objects — `order.getZip()` not `order.getCustomer().getAddress().getZip()` |

**SOLID:**

| Principle | Rule |
|---|---|
| SRP | One reason to change per class. |
| OCP | New behavior = new class, not modified existing code. |
| LSP | Subclasses must honor the parent's contract. No surprise exceptions. |
| ISP | Small focused interfaces. Don't force classes to implement what they don't use. |
| DIP | Depend on abstractions. Business logic defines the interface; implementations conform to it. |

---

## OOP Concepts

| Concept | Signal to use it | What to do |
|---|---|---|
| Encapsulation | Any class with mutable state | Private fields, expose methods, return copies of collections |
| Abstraction | Tangled logic, multiple implementations | Define an interface; hide how, expose what |
| Polymorphism | Writing `if type ==` or `switch` on types | Interface + each type handles itself |
| Inheritance | Subclasses genuinely *are* the parent, logic is identical and stable | Use it; otherwise compose |

Default: **interfaces + composition**. Inheritance only for stable, shared implementation.

---

## Design Patterns

> Most interviews use zero or one. Three or more = over-engineering.

**Creational:**

| Pattern | Use when |
|---|---|
| Factory | Callers shouldn't care which concrete class gets created |
| Builder | Object has many optional fields / constructor with 6+ params |
| Singleton | Truly one global instance needed (rare — usually wrong answer) |

**Structural:**

| Pattern | Use when |
|---|---|
| Decorator | Stack optional behaviors at runtime without subclass explosion |
| Facade | Hide internal complexity behind a simple entry point (your orchestrator already is one) |

**Behavioral:**

| Pattern | Use when |
|---|---|
| Strategy | `if/else` or `switch` on type → replace with interchangeable behavior objects |
| Observer | Multiple components react to one event — "notify," "update" in requirements |
| State Machine | Object behavior depends on current state; "state" appears repeatedly in requirements. Draw the diagram first. |

**Strategy vs Factory:** Factory decides what to *create*. Strategy decides what *behavior to use* after creation.

---

## Concurrency (Senior / Follow-up)

Threads share memory. Operations that look atomic in source are often multiple instructions. Race conditions are nondeterministic and hard to reproduce.

**Three problem types:**

| Type | What breaks | Fix |
|---|---|---|
| Correctness | Shared state corrupted — two threads both see "available" | Lock or atomic around the check-then-act |
| Coordination | Threads need to hand off work or wait efficiently | Blocking queue |
| Scarcity | Limited resources, too many requesters | Semaphore or resource pool |

**Primitives:**

| Primitive | Use for |
|---|---|
| Lock / Mutex | Protecting any shared state |
| Atomic | Single-variable counter/flag (no lock needed) |
| Semaphore | Limiting N concurrent operations |
| Blocking Queue | Producer/consumer handoff between threads |

Most questions start with Correctness. Coordination and Scarcity come as follow-ups.

---

## Things That Will Lose You Points

- Jumping into code before clarifying requirements
- Treating every noun as an entity (not every noun needs a class)
- Boolean flags for state that should be an enum
- Public fields on classes with mutable state
- `if type == "x"` chains instead of polymorphism
- Introducing patterns (Factory, Builder, etc.) where a plain class works
- Inheriting to share behavior that varies — compose instead
- Not tracing through a scenario to verify your implementation
- Building extensibility features instead of just explaining them

---

## Related
- [[> LLD Delivery Framework]]
- [[> LLD Design Principles]]
- [[> LLD OOP Concepts]]
- [[> LLD Design Patterns]]
- [[> Concurrency]]
