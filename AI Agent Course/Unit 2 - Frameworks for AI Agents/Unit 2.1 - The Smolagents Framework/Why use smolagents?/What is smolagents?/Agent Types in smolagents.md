Agents in `smolagents` operate as **multi-step agents**.

Each [`MultiStepAgent`](https://huggingface.co/docs/smolagents/main/en/reference/agents#smolagents.MultiStepAgent) performs:

- One thought
- One tool call and execution

In addition to using **[CodeAgent](https://huggingface.co/docs/smolagents/main/en/reference/agents#smolagents.CodeAgent)** as the primary type of agent, smolagents also supports **[ToolCallingAgent](https://huggingface.co/docs/smolagents/main/en/reference/agents#smolagents.ToolCallingAgent)**, which writes tool calls in JSON.

We will explore each agent type in more detail in the following sections.

> In smolagents, tools are defined using `@tool` decorator wrapping a Python function or the `Tool` class.

