In the Hugging Face ecosystem, there is a convenient feature called Serverless API that allows you to easily run inference on many models. There’s no installation or deployment required.

```python
import os
from huggingface_hub import InferenceClient

## You need a token from https://hf.co/settings/tokens, ensure that you select 'read' as the token type. If you run this on Google Colab, you can set it up in the "settings" tab under "secrets". Make sure to call it "HF_TOKEN"
os.environ["HF_TOKEN"]="hf_xxxxxxxxxxxxxx"

client = InferenceClient("meta-llama/Llama-3.3-70B-Instruct")
# if the outputs for next cells are wrong, the free model may be overloaded. You can also use this public endpoint that contains Llama-3.2-3B-Instruct
# client = InferenceClient("https://jc26mwg228mkj8dw.us-east-1.aws.endpoints.huggingface.cloud")
```
```python
output = client.text_generation(
    "The capital of France is",
    max_new_tokens=100,
)

print(output)
```

output:
```
Paris. The capital of France is Paris. Paris, the City of Light, is known for its stunning architecture, art museums, fashion, and romantic atmosphere. It's a must-visit destination for anyone interested in history, culture, and beauty. The Eiffel Tower, the Louvre Museum, and Notre-Dame Cathedral are just a few of the many iconic landmarks that make Paris a unique and unforgettable experience. Whether you're interested in exploring the city's charming neighborhoods, enjoying the local cuisine.
```
As seen in the LLM section, if we just do decoding, **the model will only stop when it predicts an EOS token**, and this does not happen here because this is a conversational (chat) model and **we didn’t apply the chat template it expects**.

If we now add the special tokens related to the [Llama-3.3-70B-Instruct model](https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct) that we’re using, the behavior changes and it now produces the expected EOS.

```python
prompt="""<|begin_of_text|><|start_header_id|>user<|end_header_id|>
The capital of France is<|eot_id|><|start_header_id|>assistant<|end_header_id|>"""
output = client.text_generation(
    prompt,
    max_new_tokens=100,
)

print(output)
```

output:
```
The capital of France is Paris.
```

Using the “chat” method is a much more convenient and reliable way to apply chat templates:
```python
output = client.chat.completions.create(
    messages=[
        {"role": "user", "content": "The capital of France is"},
    ],
    stream=False,
    max_tokens=1024,
)
print(output.choices[0].message.content)
```

output:
```
The capital of France is Paris.
```

The chat method is the RECOMMENDED method to use in order to ensure a smooth transition between models, but since this notebook is only educational, we will keep using the “text_generation” method to understand the details.
