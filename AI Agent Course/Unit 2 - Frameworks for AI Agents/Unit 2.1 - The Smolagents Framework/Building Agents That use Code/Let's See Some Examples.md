> You can follow the code in [this notebook](https://huggingface.co/agents-course/notebooks/blob/main/unit2/smolagents/code_agents.ipynb) that you can run using Google Colab.

Alfred is planning a party at the Wayne family mansion and needs your help to ensure everything goes smoothly. To assist him, we’ll apply what we’ve learned about how a multi-step `CodeAgent` operates.

![Alfred Party](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit2/smolagents/alfred-party.jpg)

If you haven’t installed `smolagents` yet, you can do so by running the following command:

```python
pip install smolagents -U
```

Let’s also login to the Hugging Face Hub to have access to the Serverless Inference API.

```python
from huggingface_hub import login

login()
```

### Selecting a Playlist for the Party Using smolagents

Music is an essential part of a successful party! Alfred needs some help selecting the playlist. Luckily, `smolagents` has got us covered! We can build an agent capable of searching the web using DuckDuckGo. To give the agent access to this tool, we include it in the tool list when creating the agent.

![Alfred Playlist](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit2/smolagents/alfred-playlist.jpg)

For the model, we’ll rely on `InferenceClientModel`, which provides access to Hugging Face’s [Serverless Inference API](https://huggingface.co/docs/api-inference/index). The default model is `"Qwen/Qwen2.5-Coder-32B-Instruct"`, which is performant and available for fast inference, but you can select any compatible model from the Hub.

Running an agent is quite straightforward:
```python
from smolagents import CodeAgent, DuckDuckGoSearchTool, InferenceClientModel

agent = CodeAgent(tools=[DuckDuckGoSearchTool()], model=InferenceClientModel())

agent.run("Search for the best music recommendations for a party at the Wayne's mansion.")
```

When you run this example, the output will **display a trace of the workflow steps being executed**. It will also print the corresponding Python code with the message:

```
 ─ Executing parsed code: ──────────────────────────────────────────────────────────────────────────────────────── 
  results = web_search(query="best music for a Batman party")                                                      
  print(results)                                                                                                   
 ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```


After a few steps, you’ll see the generated playlist that Alfred can use for the party! 🎵

### Using a Custom Tool to Prepare the Menu
![Alfred Menu](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit2/smolagents/alfred-menu.jpg)

Now that we have selected a playlist, we need to organize the menu for the guests. Again, Alfred can take advantage of `smolagents` to do so. Here, we use the `@tool` decorator to define a custom function that acts as a tool. We’ll cover tool creation in more detail later, so for now, we can simply run the code.

As you can see in the example below, we will create a tool using the `@tool` decorator and include it in the `tools` list.

```python
from smolagents import CodeAgent, tool, InferenceClientModel

# Tool to suggest a menu based on the occasion

def suggest_menu(occasion: str) -> str:
    """
    Suggests a menu based on the occasion.
    Args:
        occasion (str): The type of occasion for the party. Allowed values are:
                        - "casual": Menu for casual party.
                        - "formal": Menu for formal party.
                        - "superhero": Menu for superhero party.
                        - "custom": Custom menu.
    """
    if occasion == "casual":
        return "Pizza, snacks, and drinks."
    elif occasion == "formal":
        return "3-course dinner with wine and dessert."
    elif occasion == "superhero":
        return "Buffet with high-energy and healthy food."
    else:
        return "Custom menu for the butler."

# Alfred, the butler, preparing the menu for the party
agent = CodeAgent(tools=[suggest_menu], model=InferenceClientModel())

# Preparing the menu for the party
agent.run("Prepare a formal menu for the party.")
```

The agent will run for a few steps until finding the answer. Precising allowed values in the docstring helps direct agent to `occasion` argument values which exist and limit hallucinations.

The menu is ready! 🥗

### Using Python Imports Inside the Agent

We have the playlist and menu ready, but we need to check one more crucial detail: preparation time!

Alfred needs to calculate when everything would be ready if he started preparing now, in case they need assistance from other superheroes.

`smolagents` specializes in agents that write and execute Python code snippets, offering sandboxed execution for security.

**Code execution has strict security measures** - imports outside a predefined safe list are blocked by default. However, you can authorize additional imports by passing them as strings in `additional_authorized_imports`. For more details on secure code execution, see the official [guide](https://huggingface.co/docs/smolagents/tutorials/secure_code_execution).

When creating the agent, we’ll use `additional_authorized_imports` to allow for importing the `datetime` module.

```python
from smolagents import CodeAgent, InferenceClientModel
import numpy as np
import time
import datetime

agent = CodeAgent(tools=[], model=InferenceClientModel(), additional_authorized_imports=['datetime'])

agent.run(
    """
    Alfred needs to prepare for the party. Here are the tasks:
    1. Prepare the drinks - 30 minutes
    2. Decorate the mansion - 60 minutes
    3. Set up the menu - 45 minutes
    4. Prepare the music and playlist - 45 minutes

    If we start right now, at what time will the party be ready?
    """
)
```

These examples are just the beginning of what you can do with code agents, and we’re already starting to see their utility for preparing the party. You can learn more about how to build code agents in the [smolagents documentation](https://huggingface.co/docs/smolagents).

In summary, `smolagents` specializes in agents that write and execute Python code snippets, offering sandboxed execution for security. It supports both local and API-based language models, making it adaptable to various development environments.

### Sharing Our Custom Party Preparator Agent to the Hub

Wouldn’t it be **amazing to share our very own Alfred agent with the community**? By doing so, anyone can easily download and use the agent directly from the Hub, bringing the ultimate party planner of Gotham to their fingertips! Let’s make it happen! 🎉

The `smolagents` library makes this possible by allowing you to share a complete agent with the community and download others for immediate use. It’s as simple as the following:

```python
# Change to your username and repo name
agent.push_to_hub('sergiopaniego/AlfredAgent')
```

To download the agent again, use the code below:
```
# Change to your username and repo name
alfred_agent = agent.from_hub('sergiopaniego/AlfredAgent', trust_remote_code=True)

alfred_agent.run("Give me the best playlist for a party at Wayne's mansion. The party idea is a 'villain masquerade' theme")
```

What’s also exciting is that shared agents are directly available as Hugging Face Spaces, allowing you to interact with them in real-time. You can explore other agents [here](https://huggingface.co/spaces/davidberenstein1957/smolagents-and-tools).

You may be wondering—how did Alfred build such an agent using `smolagents`? By integrating several tools, he can generate an agent as follows. Don’t worry about the tools for now, as we’ll have a dedicated section later in this unit to explore that in detail:
```python
from smolagents import CodeAgent, DuckDuckGoSearchTool, FinalAnswerTool, InferenceClientModel, Tool, tool, VisitWebpageTool

def suggest_menu(occasion: str) -> str:
    """
    Suggests a menu based on the occasion.
    Args:
        occasion: The type of occasion for the party.
    """
    if occasion == "casual":
        return "Pizza, snacks, and drinks."
    elif occasion == "formal":
        return "3-course dinner with wine and dessert."
    elif occasion == "superhero":
        return "Buffet with high-energy and healthy food."
    else:
        return "Custom menu for the butler."

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

class SuperheroPartyThemeTool(Tool):
    name = "superhero_party_theme_generator"
    description = """
    This tool suggests creative superhero-themed party ideas based on a category.
    It returns a unique party theme idea."""
    
    inputs = {
        "category": {
            "type": "string",
            "description": "The type of superhero party (e.g., 'classic heroes', 'villain masquerade', 'futuristic Gotham').",
        }
    }
    
    output_type = "string"

    def forward(self, category: str):
        themes = {
            "classic heroes": "Justice League Gala: Guests come dressed as their favorite DC heroes with themed cocktails like 'The Kryptonite Punch'.",
            "villain masquerade": "Gotham Rogues' Ball: A mysterious masquerade where guests dress as classic Batman villains.",
            "futuristic Gotham": "Neo-Gotham Night: A cyberpunk-style party inspired by Batman Beyond, with neon decorations and futuristic gadgets."
        }
        
        return themes.get(category.lower(), "Themed party idea not found. Try 'classic heroes', 'villain masquerade', or 'futuristic Gotham'.")

# Alfred, the butler, preparing the menu for the party
agent = CodeAgent(
    tools=[
        DuckDuckGoSearchTool(), 
        VisitWebpageTool(),
        suggest_menu,
        catering_service_tool,
        SuperheroPartyThemeTool(),
	FinalAnswerTool()
    ], 
    model=InferenceClientModel(),
    max_steps=10,
    verbosity_level=2
)

agent.run("Give me the best playlist for a party at the Wayne's mansion. The party idea is a 'villain masquerade' theme")
```

As you can see, we’ve created a `CodeAgent` with several tools that enhance the agent’s functionality, turning it into the ultimate party planner ready to share with the community! 🎉

Now, it’s your turn: build your very own agent and share it with the community using the knowledge we’ve just learned! 🕵️‍♂️💡

### Inspecting Our Party Preparator Agent with OpenTelemetry and Langfuse 📡

As Alfred fine-tunes the Party Preparator Agent, he’s growing weary of debugging its runs. Agents, by nature, are unpredictable and difficult to inspect. But since he aims to build the ultimate Party Preparator Agent and deploy it in production, he needs robust traceability for future monitoring and analysis.

Once again, `smolagents` comes to the rescue! It embraces the [OpenTelemetry](https://opentelemetry.io/) standard for instrumenting agent runs, allowing seamless inspection and logging. With the help of [Langfuse](https://langfuse.com/) and the `SmolagentsInstrumentor`, Alfred can easily track and analyze his agent’s behavior.

Setting it up is straightforward!

First, we need to install the necessary dependencies:
```
pip install opentelemetry-sdk opentelemetry-exporter-otlp openinference-instrumentation-smolagents
```

Next, Alfred has already created an account on Langfuse and has his API keys ready. If you haven’t done so yet, you can sign up for Langfuse Cloud [here](https://cloud.langfuse.com/) or explore [alternatives](https://huggingface.co/docs/smolagents/tutorials/inspect_runs).

Once you have your API keys, they need to be properly configured as follows:

```python
import os
import base64

LANGFUSE_PUBLIC_KEY="pk-lf-..."
LANGFUSE_SECRET_KEY="sk-lf-..."
LANGFUSE_AUTH=base64.b64encode(f"{LANGFUSE_PUBLIC_KEY}:{LANGFUSE_SECRET_KEY}".encode()).decode()

os.environ["OTEL_EXPORTER_OTLP_ENDPOINT"] = "https://cloud.langfuse.com/api/public/otel" # EU data region
# os.environ["OTEL_EXPORTER_OTLP_ENDPOINT"] = "https://us.cloud.langfuse.com/api/public/otel" # US data region
os.environ["OTEL_EXPORTER_OTLP_HEADERS"] = f"Authorization=Basic {LANGFUSE_AUTH}"
```

Finally, Alfred is ready to initialize the `SmolagentsInstrumentor` and start tracking his agent’s performance.

```python
from opentelemetry.sdk.trace import TracerProvider

from openinference.instrumentation.smolagents import SmolagentsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace.export import SimpleSpanProcessor

trace_provider = TracerProvider()
trace_provider.add_span_processor(SimpleSpanProcessor(OTLPSpanExporter()))

SmolagentsInstrumentor().instrument(tracer_provider=trace_provider)
```

Alfred is now connected 🔌! The runs from `smolagents` are being logged in Langfuse, giving him full visibility into the agent’s behavior. With this setup, he’s ready to revisit previous runs and refine his Party Preparator Agent even further.

```python
from smolagents import CodeAgent, InferenceClientModel

agent = CodeAgent(tools=[], model=InferenceClientModel())
alfred_agent = agent.from_hub('sergiopaniego/AlfredAgent', trust_remote_code=True)
alfred_agent.run("Give me the best playlist for a party at Wayne's mansion. The party idea is a 'villain masquerade' theme")
```

