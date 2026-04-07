---
tags: [lld, design-patterns, creational, structural, behavioral]
aliases: [Design Patterns, LLD Patterns]
---

# LLD Design Patterns

Reusable building blocks for common design problems. Names for structures that emerge naturally from [[> LLD Design Principles|good design principles]].

> [!warning] Don't force patterns
> Most interview designs use zero or one pattern. Reaching for three or more is a sign of over-engineering. Patterns arise from good decisions — they don't drive them.

---

## The Eight That Matter

GoF defined 23 patterns in 1994. Modern interviews care about ~8.

| Category   | Pattern            | Use when...                                                          |
| ---------- | ------------------ | -------------------------------------------------------------------- |
| Creational | [[#Factory]]       | Callers shouldn't care which concrete class gets created             |
| Creational | [[#Builder]]       | Object has lots of optional fields or messy construction             |
| Creational | [[#Singleton]]     | You truly need one global instance (rare)                            |
| Structural | [[#Decorator]]     | Layer optional behaviors at runtime without subclass explosion       |
| Structural | [[#Facade]]        | Hide internal complexity behind a simple entry point                 |
| Behavioral | [[#Strategy]]      | Replace if/else logic with interchangeable behaviors                 |
| Behavioral | [[#Observer]]      | Multiple components react to a single event                          |
| Behavioral | [[#State Machine]] | Object behavior depends on current state and transitions get complex |

---

## Creational Patterns

### Factory

A helper that creates the right object so callers don't have to decide. Centralizes creation logic.

**Signal:** requirements say "support multiple notification types" or "handle multiple payment methods."

```python
class NotificationFactory:
    @staticmethod
    def create(notification_type: str) -> Notification:
        if notification_type == "email":
            return EmailNotification()
        elif notification_type == "sms":
            return SMSNotification()
        raise ValueError("Unknown type")
```

Adding push notifications = modify the factory in one place. Everything else stays untouched.

```python
# Another example: Vehicle factory
class VehicleFactory:
    @staticmethod
    def create(vehicle_type: str) -> Vehicle:
        if vehicle_type == "car":
            return Car()
        elif vehicle_type == "bike":
            return Bike()
        elif vehicle_type == "truck":
            return Truck()
        raise ValueError("Unknown vehicle type")
```

> [!tip] Simple Factory vs GoF Factory Method
> What you'll actually build and what interviewers expect is the Simple Factory above. The GoF version uses abstract factory classes with overriding subclasses — more complex, rarely seen in practice.

> [!warning] Polarizing pattern
> Very common in Java — some engineers see it as overengineering in other languages. Read your interviewer.

---

### Builder

Constructs a complex object step by step. Makes construction readable and handles optional fields cleanly.

**Signal:** object has many optional parameters, or a constructor with 6+ arguments where half would be `None`.

```python
request = (HttpRequest.Builder()
    .url("https://api.example.com")
    .method("POST")
    .header("Content-Type", "application/json")
    .body('{"key": "value"}')
    .build())  # validates required fields here
```

```python
# Another example: SQL query builder
query = (QueryBuilder()
    .select("id", "name", "email")
    .from_table("users")
    .where("age > 18")
    .order_by("name")
    .limit(50)
    .build())  # → "SELECT id, name, email FROM users WHERE age > 18 ORDER BY name LIMIT 50"
```

> [!warning] Niche use case
> Only reaches for this when the problem explicitly involves complex objects with lots of optional details — API clients, query builders, config objects. Simple domain objects with 2-4 fields don't need it.

---

### Singleton

Ensures only one instance of a class exists across the entire system.

**Signal:** shared resource with a single owner — config manager, connection pool, logger.

```python
class DatabaseConnection:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

```python
# Another example: App config loaded once at startup
class AppConfig:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.settings = cls._load_from_env()
        return cls._instance

config1 = AppConfig()
config2 = AppConfig()
assert config1 is config2  # same object
```

> [!warning] Usually the wrong answer
> Singletons hide dependencies and make testing harder. Passing shared objects through constructors is clearer. In Python, module-level variables are natural singletons anyway.
> 
> If an interviewer asks "should this be a Singleton?" — the answer is usually **no**, unless they explicitly want a single shared instance system-wide.

---

## Structural Patterns

### Decorator

Wraps an object to add behavior at runtime without changing the underlying class or creating subclass explosions.

**Signal:** "add logging to specific operations," "optional features," "stack behaviors," "combine enhancements."

```python
source = FileDataSource("data.txt")
source = EncryptionDecorator(source)   # wraps file source
source = CompressionDecorator(source)  # wraps encrypted source
source.write_data("sensitive info")
# data → compressed → encrypted → written to file
```

Each decorator adds one thing. Stack in any order. Add or remove without touching the base class.

```python
# Another example: Coffee order pricing
coffee = SimpleCoffee()            # $1.00
coffee = MilkDecorator(coffee)     # +$0.25
coffee = SugarDecorator(coffee)    # +$0.10
coffee = WhipDecorator(coffee)     # +$0.50
print(coffee.cost())               # $1.85
print(coffee.description())        # "Coffee, Milk, Sugar, Whip"
```

> [!tip] Decorator vs Subclass
> - **Runtime condition** (add logging only in debug mode) → Decorator
> - **Fixed type difference** (a PremiumAccount is always different) → Subclass

> [!warning] Python naming confusion
> This is the Decorator *design pattern* (object composition). Python's `@decorator` syntax is a separate language feature — different thing, same name.

---

### Facade

A coordinator that hides internal complexity behind a clean interface. You're probably already building these.

Your `Game` class in Tic Tac Toe is a facade. Any orchestrator that coordinates multiple components behind a simple API is a facade.

```python
# Caller doesn't know about Board, Player, or GameState internals
game = Game()
game.make_move(0, 0)
game.make_move(1, 1)
```

```python
# Another example: Home theater facade hides Projector, Amplifier, Lights, DVD
class HomeTheater:
    def watch_movie(self, movie: str):
        self.lights.dim(10)
        self.projector.on()
        self.amplifier.set_volume(5)
        self.dvd.play(movie)

    def end_movie(self):
        self.dvd.stop()
        self.projector.off()
        self.lights.on()

theater = HomeTheater()
theater.watch_movie("Inception")  # caller touches one object
```

> [!tip] You don't need to name it
> Build good orchestrators naturally. Mention "Facade" if it helps communicate — but the pattern is most useful when wrapping an existing messy subsystem with awkward APIs, not when designing clean from scratch.

---

## Behavioral Patterns

### Strategy

Replaces conditional logic with polymorphism. Swap behavior at runtime through composition.

**The most common pattern in LLD interviews.** Directly tests whether you understand [[> LLD OOP Concepts#Polymorphism|polymorphism]] and composition over inheritance.

**Signal:** a pile of `if/else` or `switch` statements based on type.

```python
cart = ShoppingCart()

cart.set_payment_strategy(CreditCardPayment("1234-5678"))
cart.checkout(100.00)

cart.set_payment_strategy(PayPalPayment("user@example.com"))
cart.checkout(50.00)
```

No `if paymentType == "credit"` anywhere in `checkout`. Each strategy handles itself.

```python
# Another example: Sorting strategy
sorter = DataSorter()

sorter.set_strategy(QuickSort())
sorter.sort([5, 2, 8, 1])  # uses quicksort

sorter.set_strategy(MergeSort())
sorter.sort([5, 2, 8, 1])  # uses mergesort — same interface, different algorithm
```

> [!tip] Strategy vs Factory
> **Factory** — decides *which object to create* (one-time, at instantiation).
> **Strategy** — decides *which behavior to use* after the object already exists (swappable at runtime).

---

### Observer

Objects subscribe to events and get notified when something happens. Decouples the thing that changes from the things that care.

**Signal:** requirements use the words "notify" or "update multiple components." Multiple parts of the system care about one state change.

```python
stock = Stock("AAPL")
stock.attach(PriceDisplay())
stock.attach(PriceAlert(threshold=150.00))

stock.set_price(155.00)  # both observers get called automatically
```

`Stock` doesn't know what `PriceDisplay` or `PriceAlert` do with the update. It just fires.

```python
# Another example: User service notifying downstream systems on signup
user_service = UserService()
user_service.subscribe(EmailVerificationHandler())
user_service.subscribe(WelcomeBonusHandler())
user_service.subscribe(AnalyticsHandler())

user_service.register("alice@example.com")
# → all three handlers fire automatically
```

---

### State Machine

Encapsulates each state's behavior in its own class. Eliminates scattered conditionals checking current state everywhere.

**Signal:** the word "state" appears multiple times in requirements. Vending machines, document workflows, game states, order lifecycles.

> [!tip] Draw it first
> A state diagram (circles = states, arrows = transitions labeled with actions) is one of the best ways to communicate a state machine design. Interviewers appreciate the visual — draw it before writing code.

```
NoCoinState  →(insert_coin)→  HasCoinState  →(select_product)→  DispenseState
     ↑                                                                  |
     └──────────────────────(dispense)──────────────────────────────────┘
```

Each state class handles all actions. Invalid actions print an error and don't transition. No giant switch statements anywhere.

```python
# Another example: Order lifecycle
# States: Placed → Confirmed → Shipped → Delivered
#                ↘ Cancelled (from Placed or Confirmed only)

order = Order()                  # state: Placed
order.confirm()                  # → Confirmed
order.ship()                     # → Shipped
order.cancel()                   # error: can't cancel a shipped order
order.deliver()                  # → Delivered
```

> [!tip] When it appears, it's the centerpiece
> If a state machine belongs in your solution, the whole interview is organized around it. Spend time on it.

---

## Cheat Sheet

| Pattern | Use when |
|---|---|
| Factory | Callers shouldn't care which concrete class gets created |
| Builder | Object has lots of optional fields or messy construction details |
| Singleton | You truly need one global instance (rare) |
| Decorator | Layer optional behaviors at runtime without subclass explosion |
| Facade | Hide internal complexity behind a simple entry point |
| Strategy | Replacing if/else logic with interchangeable behaviors |
| Observer | Multiple components need to react to a single event |
| State Machine | Object behavior depends on current state and transitions get messy |

---

## Related
- [[> LLD Design Principles]]
- [[> LLD OOP Concepts]]
- [[LLD - Class Design]]
- [[LLD - Implementation]]
