> In this section, we dive into the inner workings of an AI agent—its ability to reason and plan. We’ll explore how the agent leverages its internal dialogue to analyze information, break down complex problems into manageable steps, and decide what action to take next. Additionally, we introduce the ReAct approach, a prompting technique that encourages the model to think “step by step” before acting.

- Thoughts represent the **Agent’s internal reasoning and planning processes** to solve the task.
- This utilises the agent’s Large Language Model (LLM) capacity **to analyze information when presented in its prompt**.
- Think of it as the agent’s internal dialogue, where it considers the task at hand and strategizes its approach.
- The Agent’s thoughts are responsible for accessing current observations and decide what the next action(s) should be.
- Through this process, the agent can **break down complex problems into smaller, more manageable steps**, reflect on past experiences, and continuously adjust its plans based on new information.

Here are some examples of common thoughts:

|Type of Thought|Example|
|---|---|
|Planning|“I need to break this task into three steps: 1) gather data, 2) analyze trends, 3) generate report”|
|Analysis|“Based on the error message, the issue appears to be with the database connection parameters”|
|Decision Making|“Given the user’s budget constraints, I should recommend the mid-tier option”|
|Problem Solving|“To optimize this code, I should first profile it to identify bottlenecks”|
|Memory Integration|“The user mentioned their preference for Python earlier, so I’ll provide examples in Python”|
|Self-Reflection|“My last approach didn’t work well, I should try a different strategy”|
|Goal Setting|“To complete this task, I need to first establish the acceptance criteria”|
|Prioritization|“The security vulnerability should be addressed before adding new features”|

> **Note:** In the case of LLMs fine-tuned for function-calling, the thought process is optional. _In case you’re not familiar with function-calling, there will be more details in the Actions section._

## [[The ReAct Approach]]
