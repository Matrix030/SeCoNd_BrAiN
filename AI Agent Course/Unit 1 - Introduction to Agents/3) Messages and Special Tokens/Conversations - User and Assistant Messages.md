### 🧠 **What is a Conversation in Chatbots?**
Imagine you're texting a smart robot. Every time **you say something**, the robot replies back. This back-and-forth is called a **conversation**.
Example:
- You: _"I need help with my order."_
- Robot: _"Sure! What's your order number?"_
- You: _"It's ORDER-123."_

This is a **3-message conversation** between you and the robot
### 🧾 **How Does the Robot "Remember" the Chat?**

The robot doesn’t _really_ have memory like humans. So every time it responds, it needs to **see the full history** of the chat so far. This history is saved in a **list**, like a to-do list of all the messages.

```python
conversation = [
    {"role": "user", "content": "I need help with my order"},
    {"role": "assistant", "content": "Could you provide your order number?"},
    {"role": "user", "content": "It's ORDER-123"},
]
```

The robot reads this list and then figures out what to say next.