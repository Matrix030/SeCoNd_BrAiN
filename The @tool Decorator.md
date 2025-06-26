### The @tool Decorator

The `@tool` decorator is the **recommended way to define simple tools**. Under the hood, smolagents will parse basic information about the function from Python. So if you name your function clearly and write a good docstring, it will be easier for the LLM to use.

Using this approach, we define a function with:
- **A clear and descriptive function name** that helps the LLM understand its purpose.
- **Type hints for both inputs and outputs** to ensure proper usage.
- **A detailed description**, including an `Args:` section where each argument is explicitly described. These descriptions provide valuable context for the LLM, so it’s important to write them carefully.

#### Generating a tool that retrieves the highest-rated catering

![Alfred Catering](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit2/smolagents/alfred-catering.jpg)

You can follow the code in [this notebook](https://huggingface.co/agents-course/notebooks/blob/main/unit2/smolagents/tools.ipynb) that you can run using Google Colab.

Let’s imagine that Alfred has already decided on the menu for the party, but now he needs help preparing food for such a large number of guests. To do so, he would like to hire a catering service and needs to identify the highest-rated options available. Alfred can leverage a tool to search for the best catering services in his area.

Below is an example of how Alfred can use the `@tool` decorator to make this happen:
```python
from smolagents import CodeAgent, InferenceClientModel, tool

# Let's pretend we have a function that fetches the highest-rated catering services.
@tool
def catering_service_tool(query: str) -> str:
    """
    This tool returns the highest-rated catering service in Gotham City.

    Args:
        query: A search term for finding catering services.
    """
    # Example list of catering services and their ratings
    services = {
        "Gotham Catering Co.": 4.9,
        "Wayne Manor Catering": 4.8,
        "Gotham City Events": 4.7,
    }

    # Find the highest rated catering service (simulating search query filtering)
    best_service = max(services, key=services.get)

    return best_service


agent = CodeAgent(tools=[catering_service_tool], model=InferenceClientModel())

# Run the agent to find the best catering service
result = agent.run(
    "Can you give me the name of the highest-rated catering service in Gotham City?"
)

print(result)   # Output: Gotham Catering Co.
```