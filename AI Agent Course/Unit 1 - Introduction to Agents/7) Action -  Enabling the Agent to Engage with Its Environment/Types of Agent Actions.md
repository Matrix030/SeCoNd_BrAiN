There are multiple types of Agents that take actions differently:

|Type of Agent|Description|
|---|---|
|JSON Agent|The Action to take is specified in JSON format.|
|Code Agent|The Agent writes a code block that is interpreted externally.|
|Function-calling Agent|It is a subcategory of the JSON Agent which has been fine-tuned to generate a new message for each action.|

Actions themselves can serve many purposes:

|Type of Action|Description|
|---|---|
|Information Gathering|Performing web searches, querying databases, or retrieving documents.|
|Tool Usage|Making API calls, running calculations, and executing code.|
|Environment Interaction|Manipulating digital interfaces or controlling physical devices.|
|Communication|Engaging with users via chat or collaborating with other agents.|
The LLM only handles text and uses it to describe the action it wants to take and the parameters to supply to the tool. For an agent to work properly, the LLM must STOP generating new tokens after emitting all the tokens to define a complete Action. This passes control from the LLM back to the agent and ensures the result is parseable - whether the intended format is JSON, code, or function-calling.

Think of the **LLM (like ChatGPT)** as someone giving instructions to a robot assistant. This robot assistant (called an **agent**) can actually do real actions — like looking up info, opening files, or running code — but only **after** it gets clear, complete instructions.
Here’s what the passage is saying:
- The LLM **can’t do things directly**; it just **writes out the instructions in text**.
- Those instructions tell the agent **what to do** and **what details to use** (like a task and its settings).
- But for everything to work properly, the LLM has to **finish its instructions completely** and then **stop talking**.

- That pause is **important**, because it tells the agent:  
    👉 “I’m done giving you instructions — now it’s your turn to act.”
- If the LLM keeps talking after the instructions, it confuses the agent — just like if someone kept adding stuff after saying “send this email,” the assistant wouldn’t know where the message ends.

The “format” (like JSON or code) is just the way the LLM wraps the instructions so the agent can read them correctly.