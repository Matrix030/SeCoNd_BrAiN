An LLM is a type of AI model that excels at **understanding and generating human language**. They are trained on vast amounts of text data, allowing them to learn patterns, structure, and even nuance in language. These models typically consist of many millions of parameters.

Most LLMs nowadays are **built on the Transformer architecture**—a deep learning architecture based on the “Attention” algorithm, that has gained significant interest since the release of BERT from Google in 2018.

![Transformer](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/transformer.jpg)

The original Transformer architecture looked like this, with an encoder on the left and a decoder on the right.

There are 3 types of transformers:

1. **Encoders**  
    An encoder-based Transformer takes text (or other data) as input and outputs a dense representation (or embedding) of that text.
    
    - **Example**: BERT from Google
    - **Use Cases**: Text classification, semantic search, Named Entity Recognition
    - **Typical Size**: Millions of parameters
2. **Decoders**  
    A decoder-based Transformer focuses **on generating new tokens to complete a sequence, one token at a time**.
    
    - **Example**: Llama from Meta
    - **Use Cases**: Text generation, chatbots, code generation
    - **Typical Size**: Billions (in the US sense, i.e., 10^9) of parameters
3. **Seq2Seq (Encoder–Decoder)**  
    A sequence-to-sequence Transformer _combines_ an encoder and a decoder. The encoder first processes the input sequence into a context representation, then the decoder generates an output sequence.
    
    - **Example**: T5, BART
    - **Use Cases**: Translation, Summarization, Paraphrasing
    - **Typical Size**: Millions of parameters

Although Large Language Models come in various forms, LLMs are typically decoder-based models with billions of parameters. Here are some of the most well-known LLMs:

|**Model**|**Provider**|
|---|---|
|**Deepseek-R1**|DeepSeek|
|**GPT4**|OpenAI|
|**Llama 3**|Meta (Facebook AI Research)|
|**SmolLM2**|Hugging Face|
|**Gemma**|Google|
|**Mistral**|Mistral|

The underlying principle of an LLM is simple yet highly effective: **its objective is to predict the next token, given a sequence of previous tokens**. A “token” is the unit of information an LLM works with. You can think of a “token” as if it was a “word”, but for efficiency reasons LLMs don’t use whole words.

For example, while English has an estimated 600,000 words, an LLM might have a vocabulary of around 32,000 tokens (as is the case with Llama 2). Tokenization often works on sub-word units that can be combined.

For instance, consider how the tokens “interest” and “ing” can be combined to form “interesting”, or “ed” can be appended to form “interested.”

Each LLM has some **special tokens** specific to the model. The LLM uses these tokens to open and close the structured components of its generation. For example, to indicate the start or end of a sequence, message, or response. Moreover, the input prompts that we pass to the model are also structured with special tokens. The most important of those is the **End of sequence token** (EOS).

The forms of special tokens are highly diverse across model providers.

The table below illustrates the diversity of special tokens.

|**Model**|**Provider**|**EOS Token**|**Functionality**|
|---|---|---|---|
|**GPT4**|OpenAI|`<\|endoftext\|>`|End of message text|
|**Llama 3**|Meta (Facebook AI Research)|`<\|eot_id\|>`|End of sequence|
|**Deepseek-R1**|DeepSeek|`<\|end_of_sentence\|>`|End of message text|
|**SmolLM2**|Hugging Face|`<\|im_end\|>`|End of instruction or message|
|**Gemma**|Google|`<end_of_turn>`|End of conversation turn|
If you want to know more about special tokens, you can check out the configuration of the model in its Hub repository. For example, you can find the special tokens of the SmolLM2 model in its [tokenizer_config.json](https://huggingface.co/HuggingFaceTB/SmolLM2-135M-Instruct/blob/main/tokenizer_config.json).