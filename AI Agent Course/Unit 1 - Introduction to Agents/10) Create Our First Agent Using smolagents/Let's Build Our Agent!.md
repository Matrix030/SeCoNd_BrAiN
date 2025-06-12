To start, duplicate this Space: [https://huggingface.co/spaces/agents-course/First_agent_template](https://huggingface.co/spaces/agents-course/First_agent_template)

Duplicating this space means **creating a local copy on your own profile**:

![Duplicate](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/duplicate-space.gif)

After duplicating the Space, you’ll need to add your Hugging Face API token so your agent can access the model API:

1. First, get your Hugging Face token from [https://hf.co/settings/tokens](https://hf.co/settings/tokens) with permission for inference, if you don’t already have one
2. Go to your duplicated Space and click on the **Settings** tab
3. Scroll down to the **Variables and Secrets** section and click **New Secret**
4. Create a secret with the name `HF_TOKEN` and paste your token as the value
5. Click **Save** to store your token securely

Throughout this lesson, the only file you will need to modify is the (currently incomplete) **“app.py”**. You can see here the [original one in the template](https://huggingface.co/spaces/agents-course/First_agent_template/blob/main/app.py). To find yours, go to your copy of the space, then click the `Files` tab and then on `app.py` in the directory listing.

Let’s break down the code together:

- The file begins with some simple but necessary library imports
```python
from smolagents import CodeAgent, DuckDuckGoSearchTool, FinalAnswerTool, InferenceClientModel, load_tool, tool
import datetime
import requests
import pytz
import yaml
```

As outlined earlier, we will directly use the **CodeAgent** class from **smolagents**.

### The Tools

Now let’s get into the tools! If you want a refresher about tools, don’t hesitate to go back to the [[> What are Tools]] section of the course.

```python
@tool
def my_custom_tool(arg1:str, arg2:int)-> str: # it's important to specify the return type
    # Keep this format for the tool description / args description but feel free to modify the tool
    """A tool that does nothing yet 
    Args:
        arg1: the first argument
        arg2: the second argument
    """
    return "What magic will you build ?"

@tool
def get_current_time_in_timezone(timezone: str) -> str:
    """A tool that fetches the current local time in a specified timezone.
    Args:
        timezone: A string representing a valid timezone (e.g., 'America/New_York').
    """
    try:
        # Create timezone object
        tz = pytz.timezone(timezone)
        # Get current time in that timezone
        local_time = datetime.datetime.now(tz).strftime("%Y-%m-%d %H:%M:%S")
        return f"The current local time in {timezone} is: {local_time}"
    except Exception as e:
        return f"Error fetching time for timezone '{timezone}': {str(e)}"
```

The Tools are what we are encouraging you to build in this section! We give you two examples:

1. A **non-working dummy Tool** that you can modify to make something useful.
2. An **actually working Tool** that gets the current time somewhere in the world.

To define your tool it is important to:
1. Provide input and output types for your function, like in `get_current_time_in_timezone(timezone: str) -> str:`
2. **A well formatted docstring**. `smolagents` is expecting all the arguments to have a **textual description in the docstring**.
### The Agent
It uses [`Qwen/Qwen2.5-Coder-32B-Instruct`](https://huggingface.co/Qwen/Qwen2.5-Coder-32B-Instruct) as the LLM engine. This is a very capable model that we’ll access via the serverless API.

```python
final_answer = FinalAnswerTool()
model = InferenceClientModel(
    max_tokens=2096,
    temperature=0.5,
    model_id='Qwen/Qwen2.5-Coder-32B-Instruct',
    custom_role_conversions=None,
)

with open("prompts.yaml", 'r') as stream:
    prompt_templates = yaml.safe_load(stream)
    
# We're creating our CodeAgent
agent = CodeAgent(
    model=model,
    tools=[final_answer], # add your tools here (don't remove final_answer)
    max_steps=6,
    verbosity_level=1,
    grammar=None,
    planning_interval=None,
    name=None,
    description=None,
    prompt_templates=prompt_templates
)

GradioUI(agent).launch()
```

This Agent still uses the `InferenceClient` we saw in an earlier section behind the **InferenceClientModel** class!

We will give more in-depth examples when we present the framework in Unit 2. For now, you need to focus on **adding new tools to the list of tools** using the `tools` parameter of your Agent.
For example, you could use the `DuckDuckGoSearchTool` that was imported in the first line of the code, or you can examine the `image_generation_tool` that is loaded from the Hub later in the code.

**Adding tools will give your agent new capabilities**, try to be creative here!
### The System Prompt
The agent’s system prompt is stored in a separate `prompts.yaml` file. This file contains predefined instructions that guide the agent’s behavior.

Storing prompts in a YAML file allows for easy customization and reuse across different agents or use cases.

You can check the [Space’s file structure](https://huggingface.co/spaces/agents-course/First_agent_template/tree/main) to see where the `prompts.yaml` file is located and how it’s organized within the project.

The complete “app.py”:
```python
from smolagents import CodeAgent, DuckDuckGoSearchTool, InferenceClientModel, load_tool, tool
import datetime
import requests
import pytz
import yaml
from tools.final_answer import FinalAnswerTool

from Gradio_UI import GradioUI

# Below is an example of a tool that does nothing. Amaze us with your creativity!

def my_custom_tool(arg1:str, arg2:int)-> str: # it's important to specify the return type
    # Keep this format for the tool description / args description but feel free to modify the tool
    """A tool that does nothing yet 
    Args:
        arg1: the first argument
        arg2: the second argument
    """
    return "What magic will you build ?"

def get_current_time_in_timezone(timezone: str) -> str:
    """A tool that fetches the current local time in a specified timezone.
    Args:
        timezone: A string representing a valid timezone (e.g., 'America/New_York').
    """
    try:
        # Create timezone object
        tz = pytz.timezone(timezone)
        # Get current time in that timezone
        local_time = datetime.datetime.now(tz).strftime("%Y-%m-%d %H:%M:%S")
        return f"The current local time in {timezone} is: {local_time}"
    except Exception as e:
        return f"Error fetching time for timezone '{timezone}': {str(e)}"

final_answer = FinalAnswerTool()
model = InferenceClientModel(
    max_tokens=2096,
    temperature=0.5,
    model_id='Qwen/Qwen2.5-Coder-32B-Instruct',
    custom_role_conversions=None,
)

# Import tool from Hub
image_generation_tool = load_tool("agents-course/text-to-image", trust_remote_code=True)

# Load system prompt from prompt.yaml file
with open("prompts.yaml", 'r') as stream:
    prompt_templates = yaml.safe_load(stream)
    
agent = CodeAgent(
    model=model,
    tools=[final_answer], # add your tools here (don't remove final_answer)
    max_steps=6,
    verbosity_level=1,
    grammar=None,
    planning_interval=None,
    name=None,
    description=None,
    prompt_templates=prompt_templates # Pass system prompt to CodeAgent
)


GradioUI(agent).launch()
```
