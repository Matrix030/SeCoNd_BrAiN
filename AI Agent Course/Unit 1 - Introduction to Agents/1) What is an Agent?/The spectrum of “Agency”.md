Following this definition, Agents exist on a continuous spectrum of increasing agency:

|Agency Level|Description|What that’s called|Example pattern|
|---|---|---|---|
|☆☆☆|Agent output has no impact on program flow|Simple processor|`process_llm_output(llm_response)`|
|★☆☆|Agent output determines basic control flow|Router|`if llm_decision(): path_a() else: path_b()`|
|★★☆|Agent output determines function execution|Tool caller|`run_function(llm_chosen_tool, llm_chosen_args)`|
|★★★|Agent output controls iteration and program continuation|Multi-step Agent|`while llm_should_continue(): execute_next_step()`|
|★★★|One agentic workflow can start another agentic workflow|Multi-Agent|`if llm_trigger(): execute_agent()`|
Table from [smolagents conceptual guide](https://huggingface.co/docs/smolagents/conceptual_guides/intro_agents). [[smolagents conceptual guide]]
