The easiest way to ensure your LLM receives a conversation correctly formatted is to use the `chat_template` from the model’s tokenizer.
```python
messages = [
    {"role": "system", "content": "You are an AI assistant with access to various tools."},
    {"role": "user", "content": "Hi !"},
    {"role": "assistant", "content": "Hi human, what can help you with ?"},
]
```

To convert the previous conversation into a prompt, we load the tokenizer and call `apply_chat_template`:
```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("HuggingFaceTB/SmolLM2-1.7B-Instruct")
rendered_prompt = tokenizer.apply_chat_template(messages, tokenize=False, add_generation_prompt=True)
```
The `rendered_prompt` returned by this function is now ready to use as the input for the model you chose!

> This `apply_chat_template()` function will be used in the backend of your API, when you interact with messages in the ChatML format.

Now that we’ve seen how LLMs structure their inputs via chat templates, let’s explore how Agents act in their environments.

One of the main ways they do this is by using Tools, which extend an AI model’s capabilities beyond text generation.

We’ll discuss messages again in upcoming units, but if you want a deeper dive now, check out:

- [Hugging Face Chat Templating Guide](https://huggingface.co/docs/transformers/main/en/chat_templating)
- [Transformers Documentation](https://huggingface.co/docs/transformers)
- 