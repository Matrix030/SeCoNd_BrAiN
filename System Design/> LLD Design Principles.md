---
tags: [lld, design-principles, solid, oop]
aliases: [LLD Principles, Design Principles]
---

# LLD Design Principles

Principles that guide decision-making in LLD interviews — when to split a class, use inheritance, or add abstraction.

---

> [!tip] Don't memorize, apply
> Interviewers care that you apply the reasoning, not that you can recite acronyms.

---

## Two Categories

| Category | Principles |
|---|---|
| [[#General Principles]] | KISS, DRY, YAGNI, Separation of Concerns, Law of Demeter |
| [[#SOLID Principles]] | SRP, OCP, LSP, ISP, DIP |

---

## General Principles

### KISS — Keep It Simple, Stupid
Start with the simplest solution that works. Add complexity only when simplicity stops working.

> [!warning] Most violated principle in LLD interviews
> Candidates force factories, builders, and decorators where a basic class would do. Interviewers notice.

**When to add complexity:** your class hits 500 lines with 10 responsibilities, or adding a feature requires changing 5 places.

---

### DRY — Don't Repeat Yourself
Same logic in multiple places? Pull it into one place. One fix, one update.

> [!warning] Don't over-apply DRY
> If two pieces of code look similar but serve different purposes, duplication may be fine. Forced sharing creates artificial coupling.

**DRY vs KISS conflict** — sometimes the simplest solution *is* duplication. Show you understand the tradeoff:
> *"I'll keep this validation in the User class for now. If it appears 3-4 more times, we pull it into a shared validator."*

---

### YAGNI — You Aren't Gonna Need It
Build for now, not hypothetical futures. Don't add valet parking to a parking lot unless it's in requirements.

> [!tip] Design with extension in mind, but only implement what's needed.
> When the interviewer asks "how would you extend this?" — that's when you *talk* about future changes, not build them.

---

### Separation of Concerns
Different parts of code handle different responsibilities and don't know each other's internals.

**Bad** — display logic, input handling, and game rules all in one method.
**Good** — `Board`, `Display`, `InputHandler` each own their slice.

Benefit: each change is isolated. Switch from console to GUI? Only touch `InputHandler`. New win condition? Only touch `Board`.

---

### Law of Demeter
A method should only talk to its immediate friends — don't reach through objects.

```python
# Bad — knows the internals of 3 objects
order.getCustomer().getAddress().getZipCode()

# Good — Order handles the navigation internally
order.getCustomerZipCode()
```

> [!warning] Not about method chaining
> `builder.setName("John").setAge(30).build()` is fine — it returns the same type. The issue is traversing through *multiple different object types*.

---

## SOLID Principles

> [!tip] SOLID context
> These come from Java's era of deep inheritance. Modern languages favor composition over class hierarchies. Apply when the problem calls for it — don't force them and break [[> LLD Design Principles#KISS — Keep It Simple, Stupid|KISS]].

---

### SRP — Single Responsibility Principle
A class should have **one reason to change**.

**Bad:** `Report` handles content, PDF formatting, and file storage.
**Good:** `Report`, `PDFPrinter`, `FileStorage` — each owns one concern.

---

### OCP — Open/Closed Principle
Open for extension, closed for modification. Add new behavior by writing new classes, not changing existing ones.

**Bad:** `PaymentProcessor` with `if payment_type == "credit" / "paypal"` — adding crypto means modifying this method.
**Good:** `PaymentMethod` abstract class → `CreditCardPayment`, `PayPalPayment`, `CryptoPayment`. Adding crypto = new class, zero changes to `PaymentProcessor`.

---

### LSP — Liskov Substitution Principle
Subclasses must work wherever the base class works. A `Penguin` that throws `NotImplementedError` on `fly()` breaks code expecting any `Bird` to fly.

> [!warning] Red flags for LSP violation
> - Subclass throws exception for a method the parent provides
> - Callers need `if isinstance(obj, SubClass)` special-casing

**Fix:** move `fly()` to a `FlyingBird` subclass. `Penguin` extends `Bird` directly.

---

### ISP — Interface Segregation Principle
Prefer small, focused interfaces over fat ones. Don't force classes to implement methods they'll never use.

**Bad:** `Robot` implements `Worker` which includes `eat()` and `sleep()`.
**Good:** Split into `Workable`, `Feedable`, `Restable`. `Robot` only implements `Workable`.

---

### DIP — Dependency Inversion Principle
Depend on abstractions, not concrete implementations. Define the interface based on what *your business logic needs*, then have implementations conform to it.

**Bad:** `NotificationService` creates `EmailSender()` directly — can't test without real emails, can't swap to SMS.
**Good:** `NotificationService` takes a `MessageSender` interface via constructor — inject a mock for tests, swap `EmailSender` for `SmsSender` freely.

> [!tip] DIP vs Dependency Injection
> **DIP** is the principle (depend on abstractions). **Dependency injection** (passing deps via constructor) is one technique to achieve it. Related but not the same.

---

## Cheat Sheet

| Principle | Rule |
|---|---|
| KISS | Start simple, add complexity only when needed |
| DRY | Reduce duplication, simplify maintenance |
| YAGNI | Build for today, not hypothetical futures |
| Separation of Concerns | Enable independent testing and changes |
| Law of Demeter | Reduce coupling, hide internal structure |
| SRP | One reason to change per class |
| OCP | Extend without modifying existing code |
| LSP | Subclasses must honor parent's contract |
| ISP | Small focused interfaces over fat ones |
| DIP | Depend on abstractions, not implementations |

---

## Related
- [[> LLD Delivery Framework]]
- [[LLD - Class Design]]
- [[LLD - Implementation]]
