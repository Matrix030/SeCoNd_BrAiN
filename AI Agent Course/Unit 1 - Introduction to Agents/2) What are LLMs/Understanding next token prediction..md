LLMs are said to be **autoregressive**, meaning that **the output from one pass becomes the input for the next one**. This loop continues until the model predicts the next token to be the EOS token, at which point the model can stop.

![Visual Gif of autoregressive decoding](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/AutoregressionSchema.gif)

In other words, an LLM will decode text until it reaches the EOS. But what happens during a single decoding loop?

While the full process can be quite technical for the purpose of learning agents, here’s a brief overview:

- Once the input text is **tokenized**, the model computes a representation of the sequence that captures information about the meaning and the position of each token in the input sequence.
- This representation goes into the model, which outputs scores that rank the likelihood of each token in its vocabulary as being the next one in the sequence.

![Visual Gif of decoding](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/DecodingFinal.gif)

Based on these scores, we have multiple strategies to select the tokens to complete the sentence.

- The easiest decoding strategy would be to always take the token with the maximum score.
- But there are more advanced decoding strategies. For example, _beam search_ explores multiple candidate sequences to find the one with the maximum total score–even if some individual tokens have lower scores.
[Use this link for beam search visualizer](https://huggingface.co/learn/agents-course/unit1/what-are-llms#how-are-llms-used-in-ai-agents)
