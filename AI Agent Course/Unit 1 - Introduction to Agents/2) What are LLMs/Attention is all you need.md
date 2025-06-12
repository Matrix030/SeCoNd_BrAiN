A key aspect of the Transformer architecture is **Attention**. When predicting the next word, not every word in a sentence is equally important; words like “France” and “capital” in the sentence _“The capital of France is …”_ carry the most meaning.

![Visual Gif of Attention](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/AttentionSceneFinal.gif) This process of identifying the most relevant words to predict the next token has proven to be incredibly effective.

Although the basic principle of LLMs—predicting the next token—has remained consistent since GPT-2, there have been significant advancements in scaling neural networks and making the attention mechanism work for longer and longer sequences.

If you’ve interacted with LLMs, you’re probably familiar with the term _context length_, which refers to the maximum number of tokens the LLM can process, and the maximum _attention span_ it has.