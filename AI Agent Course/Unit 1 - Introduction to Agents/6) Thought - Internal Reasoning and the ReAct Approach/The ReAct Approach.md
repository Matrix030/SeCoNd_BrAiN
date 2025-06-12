A key method is the **ReAct approach**, which is the concatenation of “Reasoning” (Think) with “Acting” (Act).

ReAct is a simple prompting technique that appends “Let’s think step by step” before letting the LLM decode the next tokens.

Indeed, prompting the model to think “**step by step**” encourages the decoding process toward next tokens **that generate a plan**, rather than a final solution, since the model is encouraged to **decompose** the problem into _sub-tasks_.

This allows the model to consider sub-steps in more detail, which in general leads to less errors than trying to generate the final solution directly.

![ReAct](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/ReAct.png)

The (d) is an example of ReAct approach where we prompt "Let's think step by step"

> We have recently seen a lot of interest for reasoning strategies. This is what's behind models like Deepseek R1 or OpenAI's o1, which have been fine-tuned to "think before answering".
> These models have been trained to always include specific _thinking_ sections (enclosed between `<think>` and `</think>` special tokens). This is not just a prompting technique like ReAct, but a training method where the model learns to generate these sections after analyzing thousands of examples that show what we expect it to do.

