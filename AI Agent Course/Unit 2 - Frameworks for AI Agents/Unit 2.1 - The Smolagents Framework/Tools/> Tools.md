As we explored in [unit 1](https://huggingface.co/learn/agents-course/unit1/tools), agents use tools to perform various actions. In `smolagents`, tools are treated as **functions that an LLM can call within an agent system**.

To interact with a tool, the LLM needs an **interface description** with these key components:

- **Name**: What the tool is called
- **Tool description**: What the tool does
- **Input types and descriptions**: What arguments the tool accepts
- **Output type**: What the tool returns

For instance, while preparing for a party at Wayne Manor, Alfred needs various tools to gather information - from searching for catering services to finding party theme ideas. Here’s how a simple search tool interface might look:

- **Name:** `web_search`
- **Tool description:** Searches the web for specific queries
- **Input:** `query` (string) - The search term to look up
- **Output:** String containing the search results

By using these tools, Alfred can make informed decisions and gather all the information needed for planning the perfect party.

Below, you can see an animation illustrating how a tool call is managed:

![Agentic pipeline from https://huggingface.co/docs/smolagents/conceptual_guides/react](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/transformers/Agent_ManimCE.gif)

[[> Tool Creation Methods]]
[[Default Toolbox]]
[[Sharing and Importing Tools]]
