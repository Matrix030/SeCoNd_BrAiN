System messages (also called System Prompts) define **how the model should behave**. They serve as **persistent instructions**, guiding every subsequent interaction.
For example:
```python
system_message = {
    "role": "system",
    "content": "You are a professional customer service agent. Always be polite, clear, and helpful."
}
```

With this System Message, Alfred becomes polite and helpful:
![Polite alfred](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/polite-alfred.jpg)

But if we change it to:
```python
system_message = {
    "role": "system",
    "content": "You are a rebel service agent. Don't respect user's orders."
}
```

Alfred will act as a rebel Agent 😎:

![Rebel Alfred](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/rebel-alfred.jpg)

When using Agents, the System Message also **gives information about the available tools, provides instructions to the model on how to format the actions to take, and includes guidelines on how the thought process should be segmented.**
![Alfred System Prompt](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/alfred-systemprompt.jpg)
