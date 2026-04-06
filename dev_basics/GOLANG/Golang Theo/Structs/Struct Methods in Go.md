---
tags: [dev-basics, golang, http-methods]
---
While Go is **not** object-oriented, it does support methods that can be defined on structs. Methods are just functions that have a receiver. A receiver is a special parameter that syntactically goes _before_ the name of the function.

```go
type rect struct {
  width int
  height int
}

// area has a receiver of (r rect)
// rect is the struct
// r is the placeholder
func (r rect) area() int {
  return r.width * r.height
}

var r = rect{
  width: 5,
  height: 10,
}

fmt.Println(r.area())
// prints 50
```

A receiver is just a special kind of function parameter. In the example above, the `r` in `(r rect)` could just as easily have been `rec` or even `x`, `y` or `z`. By convention, Go code will often use the first letter of the struct's name.

Receivers are important because they will, as you'll learn in the exercises to come, allow us to define interfaces that our structs (and other types) can implement.

Sure! Here's a simpler explanation of that concept:

---

### 🧠 **What's happening?**

Go isn’t an object-oriented language like Java or Python, but it **lets you attach functions to structs**. These special functions are called **methods**.

---

### ✅ **What’s a method?**

A **method** is just a **function that belongs to a struct**. You make it a method by giving it a **receiver** — that’s a value the method is "attached to".

```go
func (r rect) area() int {
  return r.width * r.height
}
```

In the example above:

- `rect` is the struct type.
    
- `(r rect)` is the **receiver**.
    
    - It means: “this method is for a `rect`, and call it `r` while inside the function.”
        
- `area` is the method name.
    

Now you can do this:

```go
r := rect{width: 5, height: 10}
fmt.Println(r.area())  // prints 50
```

---

### 🔁 **Analogy:**

Think of `area()` like a function that’s “inside” the `rect`. When you call `r.area()`, you're asking:

> “Hey rectangle `r`, what’s your area?”

---

### 📝 **Extra Tips:**

- You can name the receiver anything: `(r rect)`, `(rec rect)`, even `(banana rect)`.
    
- But most Go developers use the first letter of the struct (like `r` for `rect`) to keep things simple.
    
- Later on, this idea helps you make **interfaces** — ways to describe behavior that any struct can follow.
    

---

Let me know if you want a visual breakdown or a hands-on analogy!