---
tags: [ai-agents, course, unit-1, introduction, moc]
aliases: [“Unit 1”, “Introduction to Agents (Unit 1 - Connector)”]
---

# Unit 1 — Introduction to Agents

> [!info] Goal
> Build a solid foundation in the fundamentals of AI Agents — what they are, how they think, and how to build one.

---

## Learning Path (read in order)

### 1. What is an Agent?
[[> What is an Agent, and how does it work?]]
An agent = **Brain (LLM) + Body (Tools)**. It can reason, plan, and take action in its environment.

### 2. What are LLMs?
[[> What are LLMs]]
LLMs are the “brain” of an agent. Understand transformers, next-token prediction, and model types (encoder, decoder, seq2seq).

### 3. Messages & Special Tokens
[[> Messages and Special Tokens]]
How LLMs structure conversations — system messages, user/assistant turns, chat templates, and EOS tokens.

### 4. Tools
[[> What are Tools]]
How agents interact with the world. Tool definitions, model context protocol, and how tools are passed to LLMs.

### 5. The Think → Act → Observe Cycle
[[> Understanding AI Agents through the Thought-Action-Observation Cycle]]
The core loop every agent runs. See it in action with Alfred the agent.

### 6. Thought — Internal Reasoning (ReAct)
[[> Thought - Internal Reasoning and the ReAct Approach]]
The ReAct pattern: how agents reason step-by-step before acting.

### 7. Action — Engaging with the Environment
[[> Actions - Enabling the Agent to Engage with Its Environment]]
Types of actions: code agents vs. stop-and-parse. How agents produce structured outputs.

### 8. Observe — Feedback and Adaptation
[[> Observe - Integrating Feedback to Reflect and Adapt]]
How agents incorporate tool outputs and observations to refine their next step.

### 9. Dummy Agent Library
[[> Dummy Agent Library]]
A simplified agent implementation to demystify how agents work under the hood.

### 10. Build Your First Agent
[[> Create Our First Agent Using smolagents]]
Hands-on: build a real agent using the smolagents framework.

---

> [!tip] Core mental model
> **Agent loop:** LLM reasons → chooses a tool → tool runs → result fed back → LLM reasons again → repeat until done.

← Back to [[> AI Agent Course (Main Unit Connector)|AI Agent Course]] | Next: [[> Unit 2 - Connector|Unit 2 →]]




