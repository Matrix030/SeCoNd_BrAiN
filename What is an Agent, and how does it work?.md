
# an Agent is: an **AI model capable of reasoning, planning, and interacting with its environment**.
A formal / precise definition:
"An Agent is a system that leverages an AI model to interact with its environment in order to achieve a user-defined objective. It combines reasoning, planning, and the execution of actions (often via external tools) to fulfill tasks."
## The Big Picture: Alfred The Agent

Meet Alfred. Alfred is an **Agent**.

![This is Alfred](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/this-is-alfred.jpg)

Imagine Alfred **receives a command**, such as: “Alfred, I would like a coffee please.”

![I would like a coffee](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/coffee-please.jpg)

Because Alfred **understands natural language**, he quickly grasps our request.

Before fulfilling the order, Alfred engages in **reasoning and planning**, figuring out the steps and tools he needs to:

1. Go to the kitchen
2. Use the coffee machine
3. Brew the coffee
4. Bring the coffee back

![Reason and plan](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/reason-and-plan.jpg)

Once he has a plan, he **must act**. To execute his plan, **he can use tools from the list of tools he knows about**.

In this case, to make a coffee, he uses a coffee machine. He activates the coffee machine to brew the coffee.
![Make coffee](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/make-coffee.jpg)

Finally, Alfred brings the freshly brewed coffee to us.

![Bring coffee](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/bring-coffee.jpg)

And this is what an Agent is: an **AI model capable of reasoning, planning, and interacting with its environment**.

We call it Agent because it has _agency_, aka it has the ability to interact with the environment.

![Agent process](https://huggingface.co/datasets/agents-course/course-images/resolve/main/en/unit1/process.jpg)

Think of the Agent as having two main parts:

1. **The Brain (AI Model)**

	This is where all the thinking happens. The AI model **handles reasoning and planning**. It decides **which Actions to take based on the situation**.

2. **The Body (Capabilities and Tools)**

	This part represents **everything the Agent is equipped to do**.

The **scope of possible actions** depends on what the agent **has been equipped with**. For example, because humans lack wings, they can’t perform the “fly” **Action**, but they can execute **Actions** like “walk”, “run” ,“jump”, “grab”, and so on.

### The spectrum of “Agency”

Following this definition, Agents exist on a continuous spectrum of increasing agency:

|Agency Level|Description|What that’s called|Example pattern|
|---|---|---|---|
|☆☆☆|Agent output has no impact on program flow|Simple processor|`process_llm_output(llm_response)`|
|★☆☆|Agent output determines basic control flow|Router|`if llm_decision(): path_a() else: path_b()`|
|★★☆|Agent output determines function execution|Tool caller|`run_function(llm_chosen_tool, llm_chosen_args)`|
|★★★|Agent output controls iteration and program continuation|Multi-step Agent|`while llm_should_continue(): execute_next_step()`|
|★★★|One agentic workflow can start another agentic workflow|Multi-Agent|`if llm_trigger(): execute_agent()`|
Table from [smolagents conceptual guide](https://huggingface.co/docs/smolagents/conceptual_guides/intro_agents). [[smolagents conceptual guide]]

continue here:
https://huggingface.co/learn/agents-course/unit1/what-are-agents#the-big-picture-alfred-the-agent