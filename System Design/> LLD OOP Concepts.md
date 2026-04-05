---
tags: [lld, oop, encapsulation, abstraction, polymorphism, inheritance]
aliases: [OOP Concepts, LLD OOP]
---

# LLD OOP Concepts

The four OOP mechanisms you use to actually implement [[> LLD Design Principles|design principles]] in code.

> [!tip] Apply, don't recite
> Forget the word "polymorphism"? Fine. What matters is that you *use* interfaces when requirements say "support multiple types."

---

## The Four Concepts

| Concept | One-liner |
|---|---|
| [[#Encapsulation]] | Hide state, expose behavior |
| [[#Abstraction]] | Hide implementation behind a clear interface |
| [[#Polymorphism]] | Same call, different behavior per type |
| [[#Inheritance]] | Share stable implementation — use sparingly |

---

## Encapsulation

Keep an object's data private. Let the object control how that data changes.

**Why:** you can enforce rules inside the method — prevent negative balances, log transactions, update related state. Public fields give you no such guarantee.

**In interviews:** basic hygiene check — are fields private? Are you returning copies of collections or mutable references?

```python
# Bad — anyone can overwrite the list directly
class ParkingLot:
    def __init__(self):
        self.spots = []

# Good — spots is private, controlled via methods
class ParkingLot:
    def __init__(self):
        self._spots = []

    def park_vehicle(self, vehicle) -> bool:
        spot = self._find_available_spot(vehicle)
        if not spot:
            return False
        spot.occupy(vehicle)
        return True

    @property
    def spots(self):
        return list(self._spots)  # copy, not a reference
```

> [!tip] Rule of thumb
> Unsure whether to expose a field or write a getter? Write the getter. Returning a collection? Return a copy.

---

## Abstraction

Expose only what's essential, hide how it works. Define *what* something does, not *how*.

**Why:** callers don't need to know if you're hitting Stripe's API or a mock. They just call `process()`. Swap the implementation without touching the caller.

**When to introduce:** look for tangled logic, lots of variations, or requirements that hint at multiple approaches — those are signals an abstraction will help.

```python
# Bad — OrderService knows too much about Stripe internals
class OrderService:
    def checkout(self, order):
        stripe = StripeAPI()
        stripe.set_api_key(self.api_key)
        stripe.create_charge(order.total, order.credit_card)

# Good — depends on an abstraction, not a concrete class
class OrderService:
    def __init__(self, payment_method: PaymentMethod):
        self.payment_method = payment_method

    def checkout(self, order):
        self.payment_method.process(order.total)
```

> [!warning] Right level of abstraction
> Too vague (`doWork()`, `handleRequest()`) → meaningless. Too specific → you haven't abstracted anything. Think: *what does the caller need to do?*

---

## Polymorphism

Same method call, each object handles itself. Replaces `if type == "credit"` and `switch(vehicleType)`.

**Why:** when you add a new type, you write a new class — the caller never changes.

```python
# Bad — every new vehicle type means modifying ParkingLot
def park_vehicle(self, vehicle):
    if vehicle.type == "car":
        ...
    elif vehicle.type == "motorcycle":
        ...

# Good — each vehicle knows its own spot size
class Car(Vehicle):
    def get_required_spot_size(self): return SpotSize.REGULAR

class Motorcycle(Vehicle):
    def get_required_spot_size(self): return SpotSize.MOTORCYCLE

class ParkingLot:
    def park_vehicle(self, vehicle: Vehicle) -> bool:
        spot = self._find_spot_by_size(vehicle.get_required_spot_size())
        return spot is not None
```

> [!warning] Tradeoff to know
> Polymorphism = flexibility and extensibility, but code flows become harder to trace and debug as implementations multiply. Be ready to explain this in interviews.

**Signal:** if you're writing type checks or switch statements on an enum → use polymorphism instead.

---

## Inheritance

A subclass is a more specific version of the parent — gets its data and behavior automatically.

**The cost:** tight coupling. Changes to the parent can break every child (the "fragile base class" problem).

> [!tip] Default to interfaces + composition
> Only reach for inheritance when you genuinely need to *share stable implementation* across classes.

### When inheritance works — shared, stable logic

```python
class BankAccount:
    def deposit(self, amount): self.balance += amount
    def withdraw(self, amount): ...
    def get_balance(self): return self.balance

class SavingsAccount(BankAccount):
    def __init__(self, interest_rate): ...

class CheckingAccount(BankAccount):
    def __init__(self, overdraft_limit): ...
```

Both *are* bank accounts. The logic is identical and stable. Inheritance fits.

### When inheritance breaks — behavior variation

```python
# Bad — ElectricCar doesn't have an engine, overriding is meaningless
class Car:
    def start_engine(self): ...

class ElectricCar(Car):
    def start_engine(self): ...  # completely different logic, wrong tool
```

When behavior varies, isolate it into its own abstraction and compose it:

```python
# Good — drivetrain is injected, not inherited
class Car:
    def __init__(self, drivetrain: Drivetrain):
        self.drivetrain = drivetrain

    def start(self):
        self.drivetrain.start()
```

Want a hybrid? Inject two drivetrains. New type? New `Drivetrain` implementation. `Car` never changes.

---

## Quick Reference

| Concept | Do | Signal to use it |
|---|---|---|
| Encapsulation | Private fields, expose methods, return copies | Any class with mutable state |
| Abstraction | Define interfaces for variations | Logic is tangled or has multiple implementations |
| Polymorphism | Let objects handle themselves | You're writing `if type ==` or `switch` on types |
| Inheritance | Share stable, identical implementation | Subclasses genuinely *are* the parent, not just similar |

---

## Related
- [[> LLD Design Principles]]
- [[> LLD Delivery Framework]]
- [[LLD - Class Design]]
