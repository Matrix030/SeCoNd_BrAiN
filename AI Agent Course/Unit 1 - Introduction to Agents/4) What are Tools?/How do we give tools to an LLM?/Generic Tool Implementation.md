We create a generic `Tool` class that we can reuse whenever we need to use a tool.

> **Disclaimer:** This example implementation is fictional but closely resembles real implementations in most libraries.

```python
from typing import Callable
class Tool:
    """
    A class representing a reusable piece of code (Tool).

    Attributes:
        name (str): Name of the tool.
        description (str): A textual description of what the tool does.
        func (callable): The function this tool wraps.
        arguments (list): A list of argument.
        outputs (str or list): The return type(s) of the wrapped function.
    """
    def __init__(self,
                 name: str,
                 description: str,
                 func: Callable,
                 arguments: list,
                 outputs: str):
        self.name = name
        self.description = description
        self.func = func
        self.arguments = arguments
        self.outputs = outputs

    def to_string(self) -> str:
        """
        Return a string representation of the tool,
        including its name, description, arguments, and outputs.
        """
        args_str = ", ".join([
            f"{arg_name}: {arg_type}" for arg_name, arg_type in             self.arguments
        ])

        return (
            f"Tool Name: {self.name},"
            f" Description: {self.description},"
            f" Arguments: {args_str},"
            f" Outputs: {self.outputs}"
        )

    def __call__(self, *args, **kwargs):
        """
        Invoke the underlying function (callable) with provided         arguments.
        """
        return self.func(*args, **kwargs)
```

It may seem complicated, but if we go slowly through it we can see what it does. We define a **`Tool`** class that includes:

- **`name`** (_str_): The name of the tool.
- **`description`** (_str_): A brief description of what the tool does.
- **`function`** (_callable_): The function the tool executes.
- **`arguments`** (_list_): The expected input parameters.
- **`outputs`** (_str_ or _list_): The expected outputs of the tool.
- **`__call__()`**: Calls the function when the tool instance is invoked.
- **`to_string()`**: Converts the tool’s attributes into a textual representation.

We could create a Tool with this class using code like the following:
```python
calculator_tool = Tool(
    "calculator",                   # name
    "Multiply two integers.",       # description
    calculator,                     # function to call
    [("a", "int"), ("b", "int")],   # inputs (names and types)
    "int",                          # output
)
```

But we can also use Python’s `inspect` module to retrieve all the information for us! This is what the `@tool` decorator does.

> If you are interested, you can disclose the following section to look at the decorator implementation.

Decorator code:
```python
import inspect

def tool(func):
    """
    A decorator that creates a Tool instance from the given function.
    """
    # Get the function signature
    signature = inspect.signature(func)

    # Extract (param_name, param_annotation) pairs for inputs
    arguments = []
    for param in signature.parameters.values():
        annotation_name = (
            param.annotation.__name__
            if hasattr(param.annotation, '__name__')
            else str(param.annotation)
        )
        arguments.append((param.name, annotation_name))

    # Determine the return annotation
    return_annotation = signature.return_annotation
    if return_annotation is inspect._empty:
        outputs = "No return annotation"
    else:
        outputs = (
            return_annotation.__name__
            if hasattr(return_annotation, '__name__')
            else str(return_annotation)
        )

    # Use the function's docstring as the description (default if None)
    description = func.__doc__ or "No description provided."

    # The function name becomes the Tool name
    name = func.__name__

    # Return a new Tool instance
    return Tool(
        name=name,
        description=description,
        func=func,
        arguments=arguments,
        outputs=outputs
    )
```

Just to reiterate, with this decorator in place we can implement our tool like this:
```python
def calculator(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b

print(calculator.to_string())
```

And we can use the `Tool`’s `to_string` method to automatically retrieve a text suitable to be used as a tool description for an LLM:
```
Tool Name: calculator, Description: Multiply two integers., Arguments: a: int, b: int, Outputs: int
```

The description is **injected** in the system prompt. Taking the example with which we started this section, here is how it would look like after replacing the `tools_description`:

![System prompt for tools](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/Agent_system_prompt_tools.png)

In the [Actions](https://huggingface.co/learn/agents-course/unit1/actions) section, we will learn more about how an Agent can **Call** this tool we just created.