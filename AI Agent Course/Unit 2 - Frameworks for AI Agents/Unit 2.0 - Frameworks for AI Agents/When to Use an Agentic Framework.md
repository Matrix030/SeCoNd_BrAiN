An agentic framework is **not always needed when building an application around LLMs**. They provide flexibility in the workflow to efficiently solve a specific task, but they’re not always necessary.

Sometimes, **predefined workflows are sufficient** to fulfill user requests, and there is no real need for an agentic framework. If the approach to build an agent is simple, like a chain of prompts, using plain code may be enough. The advantage is that the developer will have **full control and understanding of their system without abstractions**.

However, when the workflow becomes more complex, such as letting an LLM call functions or using multiple agents, these abstractions start to become helpful.

Considering these ideas, we can already identify the need for some features:

- An _LLM engine_ that powers the system.
- A _list of tools_ the agent can access.
- A _parser_ for extracting tool calls from the LLM output.
- A _system prompt_ synced with the parser.
- A _memory system_.
- _Error logging and retry mechanisms_ to control LLM mistakes.

We’ll explore how these topics are resolved in various frameworks, including `smolagents`, `LlamaIndex`, and `LangGraph`.

## Agentic Frameworks Units

|Framework|Description|Unit Author|
|---|---|---|
|[smolagents](https://huggingface.co/learn/agents-course/unit2/smolagents/introduction)|Agents framework developed by Hugging Face.|Sergio Paniego - [HF](https://huggingface.co/sergiopaniego) - [X](https://x.com/sergiopaniego) - [Linkedin](https://www.linkedin.com/in/sergio-paniego-blanco)|
|[Llama-Index](https://huggingface.co/learn/agents-course/unit2/llama-index/introduction)|End-to-end tooling to ship a context-augmented AI agent to production|David Berenstein - [HF](https://huggingface.co/davidberenstein1957) - [X](https://x.com/davidberenstei) - [Linkedin](https://www.linkedin.com/in/davidberenstein)|
|[LangGraph](https://huggingface.co/learn/agents-course/unit2/langgraph/introduction)|Agents allowing stateful orchestration of agents|Joffrey THOMAS - [HF](https://huggingface.co/Jofthomas) - [X](https://x.com/Jthmas404) - [Linkedin](https://www.linkedin.com/in/joffrey-thomas)|
# Simpler Explaination

### 🤖 What's an **Agentic Framework**?
An **agentic framework** gives an AI system the _freedom_ to figure out how to solve a task by reasoning, deciding what steps to take, possibly using tools (e.g. calculators, search engines), and adapting as it goes — like a smart assistant.

### 🛠 What’s a **Predefined Workflow**?

A **predefined workflow** is a fixed, step-by-step process the AI follows to get a job done — like a recipe.
## ⚖️ Layman’s Comparison

|Scenario|Predefined Workflow (Simple Logic)|Agentic Framework (Flexible Assistant)|
|---|---|---|
|**Making Coffee**|You follow a 3-step recipe: 1) Boil water, 2) Add coffee, 3) Pour. You always do it the same way.|You tell your smart assistant: "Make me something energizing." It checks your calendar, sees a long day ahead, and decides to brew strong coffee and bring a snack.|
|**Using LLMs to Summarize Text**|You send the text and ask: “Summarize this.” LLM replies. Simple and repeatable.|You say: “Summarize this, check if there are any legal terms, and email it to my lawyer.” The agent breaks it down, uses tools (summarizer, legal glossary, email client), and figures it out.|
|**Programming Task**|Prompt chain: Ask for data, run a formula, get output — written as plain code.|Agent figures out what steps are needed, maybe even decides when to call a search function or ask follow-up questions.|
## 🧩 When Do You Need an Agentic Framework?

|Situation|Need Agentic Framework?|Why?|
|---|---|---|
|Just summarizing a paragraph|❌ No|A simple one-shot prompt works fine.|
|Planning a travel itinerary based on changing user preferences|✅ Yes|It needs reasoning, web search, and adapting on the fly.|
|Generating a SQL query from natural language|❌ Maybe not|A static prompt template might be enough.|
|Debugging code and finding missing libraries|✅ Yes|LLM needs to try, fail, retry, maybe search online.|

## 🧱 Features You Need in an Agentic Framework
Let’s compare this to building a **robot butler** who takes care of tasks:

|Feature|Analogy|Why It Matters|
|---|---|---|
|**LLM engine**|Brain of the butler|Powers reasoning and language understanding|
|**List of tools**|Coffee machine, vacuum, phone|Lets it take actions (call APIs, calculate)|
|**Parser**|Ears that understand commands|Extracts what action the LLM wants to do|
|**System prompt synced with parser**|Training manual|Ensures the butler knows what tools it has and how to use them|
|**Memory**|Notepad for past conversations|Helps the LLM remember what’s been said/done|
|**Error handling**|Safety mechanism|Prevents the agent from getting stuck or making repeated mistakes|


