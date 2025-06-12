The complete answer may seem overwhelming, but we essentially use the system prompt to provide textual descriptions of available tools to the model:

![System prompt for tools](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/Agent_system_prompt.png)

For this to work, we have to be very precise and accurate about:

1. **What the tool does**
2. **What exact inputs it expects**

This is the reason why tool descriptions are usually provided using expressive but precise structures, such as computer languages or JSON. It’s not _necessary_ to do it like that, any precise and coherent format would work.

If this seems too theoretical, let’s understand it through a concrete example.

We will implement a simplified **calculator** tool that will just multiply two integers. This could be our Python implementation:

```python
def calculator(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b
```

So our tool is called `calculator`, it **multiplies two integers**, and it requires the following inputs:

- **`a`** (_int_): An integer.
- **`b`** (_int_): An integer.

The output of the tool is another integer number that we can describe like this:

- (_int_): The product of `a` and `b`.

All of these details are important. Let’s put them together in a text string that describes our tool for the LLM to understand.

```
Tool Name: calculator, Description: Multiply two integers., Arguments: a: int, b: int, Outputs: int
```

> **Reminder:** This textual description is _what we want the LLM to know about the tool_.

When we pass the previous string as part of the input to the LLM, the model will recognize it as a tool, and will know what it needs to pass as inputs and what to expect from the output.

If we want to provide additional tools, we must be consistent and always use the same format. This process can be fragile, and we might accidentally overlook some details.

Is there a better way?

### [[Auto-formatting Tool sections]]

### [[Generic Tool Implementation]]

### [[Model Context Protocol]]

Tools play a crucial role in enhancing the capabilities of AI agents.
To summarize, we learned:
- _What Tools Are_: Functions that give LLMs extra capabilities, such as performing calculations or accessing external data.
- _How to Define a Tool_: By providing a clear textual description, inputs, outputs, and a callable function.
- _Why Tools Are Essential_: They enable Agents to overcome the limitations of static model training, handle real-time tasks, and perform specialized actions.

Now, we can move on to the [Agent Workflow](https://huggingface.co/learn/agents-course/unit1/agent-steps-and-structure) where you’ll see how an Agent observes, thinks, and acts. This **brings together everything we’ve covered so far** and sets the stage for creating your own fully functional AI Agent.