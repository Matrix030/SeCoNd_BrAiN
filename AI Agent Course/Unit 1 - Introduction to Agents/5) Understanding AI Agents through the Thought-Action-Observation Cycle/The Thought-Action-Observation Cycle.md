The three components work together in a continuous loop. To use an analogy from programming, the agent uses a **while loop**: the loop continues until the objective of the agent has been fulfilled.

Visually, it looks like this:

![Think, Act, Observe cycle](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/AgentCycle.gif)

In many Agent frameworks, **the rules and guidelines are embedded directly into the system prompt**, ensuring that every cycle adheres to a defined logic.

In a simplified version, our system prompt may look like this:

![Think, Act, Observe cycle](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/system_prompt_cycle.png)

We see here that in the System Message we defined :

- The _Agent’s behavior_.
- The _Tools our Agent has access to_, as we described in the previous section.
- The _Thought-Action-Observation Cycle_, that we bake into the LLM instructions.

Let’s take a small example to understand the process before going deeper into each step of the process.

## [[Alfred, the weather Agent]]

The following are the steps:
[[> Thought - Internal Reasoning and the ReAct Approach | Thought]]
[[> Actions - Enabling the Agent to Engage with Its Environment| Action]]
[[> Observe - Integrating Feedback to Reflect and Adapt| Observe]]

