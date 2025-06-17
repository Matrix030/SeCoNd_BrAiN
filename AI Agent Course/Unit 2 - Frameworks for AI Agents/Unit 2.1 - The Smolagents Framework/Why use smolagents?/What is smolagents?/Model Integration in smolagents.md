`smolagents` supports flexible LLM integration, allowing you to use any callable model that meets [certain criteria](https://huggingface.co/docs/smolagents/main/en/reference/models). The framework provides several predefined classes to simplify model connections:

- **[TransformersModel](https://huggingface.co/docs/smolagents/main/en/reference/models#smolagents.TransformersModel):** Implements a local `transformers` pipeline for seamless integration.
- **[InferenceClientModel](https://huggingface.co/docs/smolagents/main/en/reference/models#smolagents.InferenceClientModel):** Supports [serverless inference](https://huggingface.co/docs/huggingface_hub/main/en/guides/inference) calls through [Hugging Face’s infrastructure](https://huggingface.co/docs/api-inference/index), or via a growing number of [third-party inference providers](https://huggingface.co/docs/huggingface_hub/main/en/guides/inference#supported-providers-and-tasks).
- **[LiteLLMModel](https://huggingface.co/docs/smolagents/main/en/reference/models#smolagents.LiteLLMModel):** Leverages [LiteLLM](https://www.litellm.ai/) for lightweight model interactions.
- **[OpenAIServerModel](https://huggingface.co/docs/smolagents/main/en/reference/models#smolagents.OpenAIServerModel):** Connects to any service that offers an OpenAI API interface.
- **[AzureOpenAIServerModel](https://huggingface.co/docs/smolagents/main/en/reference/models#smolagents.AzureOpenAIServerModel):** Supports integration with any Azure OpenAI deployment.

This flexibility ensures that developers can choose the model and service most suitable for their specific use cases, and allows for easy experimentation.

Now that we understood why and when to use smolagents, let’s dive deeper into this powerful library!