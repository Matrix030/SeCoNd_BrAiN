---
tags: [ai-agents, course, unit-1, introduction, tokenization]
---
We want to **automatically generate a description of what a tool (Python function or class) does**, just by looking at the code itself.
### 🧰 How Will We Do It?
We’ll use **Python's introspection** — a fancy word that just means:
> "Python can look at its own code while it’s running."

That means we can write a Python program that **reads other Python functions** and pulls out information like:
- **Function name** → tells us what the tool is called.
- **Type hints** → tells us what kinds of inputs and outputs the tool expects.
- **Docstring** → a short explanation inside the function that tells us what it does.

### 🧪 What Do We Need From the Original Code?
To make this work, the original tool must:
1. **Use type hints** → like saying `name: str` or `age: int`.
2. **Have a docstring** → like a comment that explains what the function does.
3. **Use clear function names** → like `calculate_total()` instead of `ct()`.

If the code is written this way, then we can write another program that will **read that code** and **automatically generate** a human-readable description of the tool — like "This function takes a user's name and age and returns a greeting message."