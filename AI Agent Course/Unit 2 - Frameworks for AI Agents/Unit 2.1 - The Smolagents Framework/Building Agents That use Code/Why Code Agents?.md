In a multi-step agent process, the LLM writes and executes actions, typically involving external tool calls. Traditional approaches use a JSON format to specify tool names and arguments as strings, **which the system must parse to determine which tool to execute**.

However, research shows that **tool-calling LLMs work more effectively with code directly**. This is a core principle of `smolagents`, as shown in the diagram above from[[ Additional Stuff of Curiosity | Executable Code Actions Elicit Better LLM Agents ]].

Writing actions in code rather than JSON offers several key advantages:

- **Composability**: Easily combine and reuse actions
- **Object Management**: Work directly with complex structures like images
- **Generality**: Express any computationally possible task
- **Natural for LLMs**: High-quality code is already present in LLM training data