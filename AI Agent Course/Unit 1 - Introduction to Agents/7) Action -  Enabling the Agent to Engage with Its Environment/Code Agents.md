## Code Agents

An alternative approach is using _Code Agents_. The idea is: **instead of outputting a simple JSON object**, a Code Agent generates an **executable code block—typically in a high-level language like Python**.

![Code Agents](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/code-vs-json-actions.png)

This approach offers several advantages:

- **Expressiveness:** Code can naturally represent complex logic, including loops, conditionals, and nested functions, providing greater flexibility than JSON.
- **Modularity and Reusability:** Generated code can include functions and modules that are reusable across different actions or tasks.
- **Enhanced Debuggability:** With a well-defined programming syntax, code errors are often easier to detect and correct.
- **Direct Integration:** Code Agents can integrate directly with external libraries and APIs, enabling more complex operations such as data processing or real-time decision making.

**You must keep in mind that executing LLM-generated code may pose security risks, from prompt injection to the execution of harmful code. That’s why it’s recommended to use AI agent frameworks like `smolagents` that integrate default safeguards. If you want to know more about the risks and how to mitigate them, [please have a look at this dedicated section](https://huggingface.co/docs/smolagents/tutorials/secure_code_execution).**

For example, a Code Agent tasked with fetching the weather might generate the following Python snippet:
```python
# Code Agent Example: Retrieve Weather Information
def get_weather(city):
    import requests
    api_url = f"https://api.weather.com/v1/location/{city}?apiKey=YOUR_API_KEY"
    response = requests.get(api_url)
    if response.status_code == 200:
        data = response.json()
        return data.get("weather", "No weather information available")
    else:
        return "Error: Unable to fetch weather data."

# Execute the function and prepare the final answer
result = get_weather("New York")
final_answer = f"The current weather in New York is: {result}"
print(final_answer)
```

In this example, the Code Agent:

- Retrieves weather data **via an API call**,
- Processes the response,
- And uses the print() function to output a final answer.

This method **also follows the stop and parse approach** by clearly delimiting the code block and signaling when execution is complete (here, by printing the final_answer).