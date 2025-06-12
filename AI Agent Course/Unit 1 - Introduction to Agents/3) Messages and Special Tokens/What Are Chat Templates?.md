Think of **chat templates** like special formats for how messages are written down before giving them to the robot. Different robots like messages in different "styles."

#### 💬 Example 1: SmolLM Style (like writing a play)

```
<|im_start|>user
I need help with my order<|im_end|>
<|im_start|>assistant
Could you provide your order number?<|im_end|>
<|im_start|>user
It's ORDER-123<|im_end|>
```

It labels who is talking and marks the start and end.

#### 🧾 Example 2: Llama 3.2 Style (slightly different labels)

```
<|start_header_id|>user<|end_header_id|>
I need help with my order<|eot_id|>
```

This one also labels the user and assistant but uses different "tags" to show message boundaries.

Templates can handle complex multi-turn conversations while maintaining context:

```python
messages = [
    {"role": "system", "content": "You are a math tutor."},
    {"role": "user", "content": "What is calculus?"},
    {"role": "assistant", "content": "Calculus is a branch of mathematics..."},
    {"role": "user", "content": "Can you give me an example?"},
]
```

The `transformers` library will take care of chat templates for you as part of the tokenization process. Read more about how transformers uses chat templates [here](https://huggingface.co/docs/transformers/main/en/chat_templating#how-do-i-use-chat-templates). All we have to do is structure our messages in the correct way and the tokenizer will take care of the rest.

You can experiment with the following Space to see how the same conversation would be formatted for different models using their corresponding chat templates: [https://huggingface.co/learn/agents-course/unit1/messages-and-special-tokens#chat-templates]

## [[Messages to Prompt]]
