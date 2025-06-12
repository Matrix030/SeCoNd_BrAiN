In the last section, we learned how we can create Agents from scratch using Python code, and we **saw just how tedious that process can be**. Fortunately, many Agent libraries simplify this work by **handling much of the heavy lifting for you**.

In this tutorial, **you’ll create your very first Agent** capable of performing actions such as image generation, web search, time zone checking and much more!

You will also publish your agent **on a Hugging Face Space so you can share it with friends and colleagues**.

Let’s get started!

## What is smolagents?

![smolagents](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/smolagents.png)

To make this Agent, we’re going to use `smolagents`, a library that **provides a framework for developing your agents with ease**.

This lightweight library is designed for simplicity, but it abstracts away much of the complexity of building an Agent, allowing you to focus on designing your agent’s behavior.

We’re going to get deeper into smolagents in the next Unit. Meanwhile, you can also check this [blog post](https://huggingface.co/blog/smolagents) or the library’s [repo in GitHub](https://github.com/huggingface/smolagents).

In short, `smolagents` is a library that focuses on **codeAgent**, a kind of agent that performs **“Actions”** through code blocks, and then **“Observes”** results by executing the code.

Here is an example of what we’ll build!

We provided our agent with an **Image generation tool** and asked it to generate an image of a cat.

The agent inside `smolagents` is going to have the **same behaviors as the custom one we built previously**: it’s going **to think, act and observe in cycle** until it reaches a final answer:

Exciting, right?

[[Let's Build Our Agent!]]
