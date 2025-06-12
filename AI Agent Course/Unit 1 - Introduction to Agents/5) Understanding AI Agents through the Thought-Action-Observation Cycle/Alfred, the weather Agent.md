We created Alfred, the Weather Agent.
A user asks Alfred: “What’s the current weather in New York?”

![Alfred Agent](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/alfred-agent.jpg)

Alfred’s job is to answer this query using a weather API tool.

[[Here’s how the cycle unfolds]]

What we see in this example:
- **Agents iterate through a loop until the objective is fulfilled:**
**Alfred’s process is cyclical**. It starts with a thought, then acts by calling a tool, and finally observes the outcome. If the observation had indicated an error or incomplete data, Alfred could have re-entered the cycle to correct its approach.

- **Tool Integration:**
The ability to call a tool (like a weather API) enables Alfred to go **beyond static knowledge and retrieve real-time data**, an essential aspect of many AI Agents.

- **Dynamic Adaptation:**
Each cycle allows the agent to incorporate fresh information (observations) into its reasoning (thought), ensuring that the final answer is well-informed and accurate.

This example showcases the core concept behind the _ReAct cycle_ (a concept we’re going to develop in the next section): **the interplay of Thought, Action, and Observation empowers AI agents to solve complex tasks iteratively**.

By understanding and applying these principles, you can design agents that not only reason about their tasks but also **effectively utilize external tools to complete them**, all while continuously refining their output based on environmental feedback.