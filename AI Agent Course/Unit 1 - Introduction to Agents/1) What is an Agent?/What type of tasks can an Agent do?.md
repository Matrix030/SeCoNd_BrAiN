An Agent can perform any task we implement via **Tools** to complete **Actions**.

For example, if I write an Agent to act as my personal assistant (like Siri) on my computer, and I ask it to “send an email to my Manager asking to delay today’s meeting”, I can give it some code to send emails. This will be a new Tool the Agent can use whenever it needs to send an email. We can write it in Python:
```python
def send_message_to(recipient, message):
    """Useful to send an e-mail message to a recipient"""
    ...
```
The LLM, as we’ll see, will generate code to run the tool when it needs to, and thus fulfill the desired task.
```python
send_message_to("Manager", "Can we postpone today's meeting?")
```
The **design of the Tools is very important and has a great impact on the quality of your Agent**. Some tasks will require very specific Tools to be crafted, while others may be solved with general purpose tools like “web_search”.

_**Note that **Actions are not the same as Tools**. An Action, for instance, can involve the use of multiple Tools to complete._

Allowing an agent to interact with its environment **allows real-life usage for companies and individuals**.

### [[Example 1 - Personal Virtual Assistants]]

### [[Example 2 - Customer Service Chatbots]]

### [[Example 3 - AI Non-Playable Character in a video game]]


