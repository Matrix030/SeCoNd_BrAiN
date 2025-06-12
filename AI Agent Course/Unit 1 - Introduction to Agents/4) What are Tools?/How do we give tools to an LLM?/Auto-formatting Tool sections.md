Our tool was written in Python, and the implementation already provides everything we need:

- A descriptive name of what it does: `calculator`
- A longer description, provided by the function’s docstring comment: `Multiply two integers.`
- The inputs and their type: the function clearly expects two `int`s.
- The type of the output.

We could provide the Python source code as the _specification_ of the tool for the LLM, but the way the tool is implemented does not matter. All that matters is its name, what it does, the inputs it expects and the output it provides.

> We will leverage Python’s introspection features to leverage the source code and build a tool description automatically for us. All we need is that the tool implementation uses type hints, docstrings, and sensible function names. We will write some code to extract the relevant portions from the source code.
> [[How Will We Do It? - Simpler Explaination| Simpler Explaination]]


After we are done, we’ll only need to use a Python decorator to indicate that the `calculator` function is a tool:
```python
def calculator(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b

print(calculator.to_string())
```

Note the `@tool` decorator before the function definition.
With the implementation we’ll see next, we will be able to retrieve the following text automatically from the source code via the `to_string()` function provided by the decorator:

```
Tool Name: calculator, Description: Multiply two integers., Arguments: a: int, b: int, Outputs: int
```

As you can see, it’s the same thing we wrote manually before!