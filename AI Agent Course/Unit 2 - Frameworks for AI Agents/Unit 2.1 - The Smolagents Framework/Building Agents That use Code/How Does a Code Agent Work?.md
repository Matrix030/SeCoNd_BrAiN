![From https://huggingface.co/docs/smolagents/conceptual_guides/react](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/smolagents/codeagent_docs.png)

The diagram above illustrates how `CodeAgent.run()` operates, following the [[The ReAct Approach | ReAct]] framework we mentioned in Unit 1. The main abstraction for agents in `smolagents` is a `MultiStepAgent`, which serves as the core building block. `CodeAgent` is a special kind of `MultiStepAgent`, as we will see in an example below.

A `CodeAgent` performs actions through a cycle of steps, with existing variables and knowledge being incorporated into the agent’s context, which is kept in an execution log:
1. The system prompt is stored in a `SystemPromptStep`, and the user query is logged in a `TaskStep`.
2. Then, the following while loop is executed:
    2.1 Method `agent.write_memory_to_messages()` writes the agent’s logs into a list of LLM-readable [chat messages](https://huggingface.co/docs/transformers/main/en/chat_templating).
    2.2 These messages are sent to a `Model`, which generates a completion.
    2.3 The completion is parsed to extract the action, which, in our case, should be a code snippet since we’re working with a `CodeAgent`.
    2.4 The action is executed.
    2.5 The results are logged into memory in an `ActionStep`.
## [[Let's See Some Examples]]
