A **Tool is a function given to the LLM**. This function should fulfill a **clear objective**.

Here are some commonly used tools in AI agents:

|Tool|Description|
|---|---|
|Web Search|Allows the agent to fetch up-to-date information from the internet.|
|Image Generation|Creates images based on text descriptions.|
|Retrieval|Retrieves information from an external source.|
|API Interface|Interacts with an external API (GitHub, YouTube, Spotify, etc.).|

Those are only examples, as you can in fact create a tool for any use case!

A good tool should be something that **complements the power of an LLM**.

For instance, if you need to perform arithmetic, giving a **calculator tool** to your LLM will provide better results than relying on the native capabilities of the model.

Furthermore, **LLMs predict the completion of a prompt based on their training data**, which means that their internal knowledge only includes events prior to their training. Therefore, if your agent needs up-to-date data you must provide it through some tool.

For instance, if you ask an LLM directly (without a search tool) for today’s weather, the LLM will potentially hallucinate random weather.

![Weather](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/weather.jpg)

- A Tool should contain:
    
    - A **textual description of what the function does**.
    - A _Callable_ (something to perform an action).
    - _Arguments_ with typings.
    - (Optional) Outputs with typings.
