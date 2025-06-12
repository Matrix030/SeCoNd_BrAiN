## The Stop and Parse Approach

One key method for implementing actions is the **stop and parse approach**. This method ensures that the agent’s output is structured and predictable:

1. **Generation in a Structured Format**:
The agent outputs its intended action in a clear, predetermined format (JSON or code).

2. **Halting Further Generation**:
Once the text defining the action has been emitted, **the LLM stops generating additional tokens**. This prevents extra or erroneous output.

3. **Parsing the Output**:
An external parser reads the formatted action, determines which Tool to call, and extracts the required parameters.

For example, an agent needing to check the weather might output:
```
Thought: I need to check the current weather for New York.
Action :
{
  "action": "get_weather",
  "action_input": {"location": "New York"}
}
```

The framework can then easily parse the name of the function to call and the arguments to apply.

This clear, machine-readable format minimizes errors and enables external tools to accurately process the agent’s command.

Note: Function-calling agents operate similarly by structuring each action so that a designated function is invoked with the correct arguments. We’ll dive deeper into those types of Agents in a future Unit.

