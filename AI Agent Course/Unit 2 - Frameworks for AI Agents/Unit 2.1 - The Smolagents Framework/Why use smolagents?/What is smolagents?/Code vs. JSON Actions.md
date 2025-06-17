Unlike other frameworks where agents write actions in JSON, `smolagents` **focuses on tool calls in code**, simplifying the execution process. This is because there’s no need to parse the JSON in order to build code that calls the tools: the output can be executed directly.

The following diagram illustrates this difference:

![Code vs. JSON actions](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/transformers/code_vs_json_actions.png)

To review the difference between Code vs JSON Actions, you can revisit [[> Actions - Enabling the Agent to Engage with Its Environment | Actions]]

## Simpler Explaination:
### 🧠 **What’s happening?**

A smart assistant (like an AI agent) is trying to find the cheapest country to buy a smartphone. To do that, it needs to:

1. Look up exchange rates and taxes,
2. Get the phone's local price,
3. Convert the price to USD,
4. Add shipping,
5. Then compare all the final costs.

### 🟥 Left side: **LLM Agent using JSON Actions**
This agent talks by sending one small instruction at a time, like:
> “Hey, get me Germany’s exchange rate.”  
> “Now, get the phone price in Germany.”  
> “Now, convert it to USD with tax.”

🔁 It does this over and over — one message, one tool, one country at a time. It’s slow and takes **lots of steps** (like many text messages back and forth).
### 🟩 Right side: **LLM Agent using Code (smolagents style)**
This agent just writes a small piece of **code** that says:

> “Loop through all countries, do all the lookups and calculations, and tell me which one is cheapest.”

💡 The code handles everything **in one go** — no back-and-forth. It’s faster and smarter.

### ⚙️ Key Difference

|JSON Agent|Code Agent (smolagents)|
|---|---|
|Talks using step-by-step **text or JSON messages**|Writes and runs actual **code**|
|Needs multiple steps per country|Handles **all countries** in a single block|
|Harder to manage logic and order|Uses regular programming tools (like `for` loops and `min`)|
|Slower, more overhead|Faster, more efficient|
