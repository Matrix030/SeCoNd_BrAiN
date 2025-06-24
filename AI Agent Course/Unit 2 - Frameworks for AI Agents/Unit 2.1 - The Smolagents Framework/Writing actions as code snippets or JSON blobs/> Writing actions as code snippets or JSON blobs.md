You can follow the code in [this notebook](https://huggingface.co/agents-course/notebooks/blob/main/unit2/smolagents/tool_calling_agents.ipynb) that you can run using Google Colab.

Tool Calling Agents are the second type of agent available in `smolagents`. Unlike Code Agents that use Python snippets, these agents **use the built-in tool-calling capabilities of LLM providers** to generate tool calls as **JSON structures**. This is the standard approach used by OpenAI, Anthropic, and many other providers.

Let’s look at an example. When Alfred wants to search for catering services and party ideas, a `CodeAgent` would generate and run Python code like this:

Copied

for query in [
    "Best catering services in Gotham City", 
    "Party theme ideas for superheroes"
]:
    print(web_search(f"Search for: {query}"))

A `ToolCallingAgent` would instead create a JSON structure:

Copied

[
    {"name": "web_search", "arguments": "Best catering services in Gotham City"},
    {"name": "web_search", "arguments": "Party theme ideas for superheroes"}
]

This JSON blob is then used to execute the tool calls.

While `smolagents` primarily focuses on `CodeAgents` since [they perform better overall](https://huggingface.co/papers/2402.01030), `ToolCallingAgents` can be effective for simple systems that don’t require variable handling or complex tool calls.

![Code vs JSON Actions](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/transformers/code_vs_json_actions.png)

## How Do Tool Calling Agents Work?

Tool Calling Agents follow the same multi-step workflow as Code Agents (see the [previous section](https://huggingface.co/learn/agents-course/en/unit2/smolagents/code_agents) for details).

The key difference is in **how they structure their actions**: instead of executable code, they **generate JSON objects that specify tool names and arguments**. The system then **parses these instructions** to execute the appropriate tools.

## Example: Running a Tool Calling Agent

Let’s revisit the previous example where Alfred started party preparations, but this time we’ll use a `ToolCallingAgent` to highlight the difference. We’ll build an agent that can search the web using DuckDuckGo, just like in our Code Agent example. The only difference is the agent type - the framework handles everything else:

```python
from smolagents import ToolCallingAgent, DuckDuckGoSearchTool, InferenceClientModel

agent = ToolCallingAgent(tools=[DuckDuckGoSearchTool()], model=InferenceClientModel())

agent.run("Search for the best music recommendations for a party at the Wayne's mansion.")
```
When you examine the agent’s trace, instead of seeing `Executing parsed code:`, you’ll see something like:

```
╭─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ Calling tool: 'web_search' with arguments: {'query': "best music recommendations for a party at Wayne's         │
│ mansion"}                                                                                                       │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```

The agent generates a structured tool call that the system processes to produce the output, rather than directly executing code like a `CodeAgent`.

Now that we understand both agent types, we can choose the right one for our needs. Let’s continue exploring `smolagents` to make Alfred’s party a success! 🎉

## [](https://huggingface.co/learn/agents-course/en/unit2/smolagents/tool_calling_agents#resources)Resources

- [ToolCallingAgent documentation](https://huggingface.co/docs/smolagents/v1.8.1/en/reference/agents#smolagents.ToolCallingAgent) - Official documentation for ToolCallingAgent
