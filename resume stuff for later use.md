Great question! What you're trying to do is essentially **"set up a reusable instruction + resume context" at the beginning of the chat**, and then **reuse that setup with different job descriptions** — without repeating the resume or rules every time.

Because we're in a stateless system (i.e., there's no persistent memory between separate chats), you can **"prime"** me at the beginning of this chat with:

1. Your full resume
    
2. Your fixed instructions (rules)
    
3. Then, reuse that setup in **this chat session** as many times as you want by just giving new job descriptions
    

---

### ✅ Step-by-step: How to prompt me

#### 🔹 Step 1: Initial prompt — set the rule and provide your resume

You start the chat with a message like:

```
From this point onward, here are the rules for this chat:

- I will give you my resume below. This resume will remain the same throughout this session.
- For each job description I provide after this, you must:
  - Analyze the job description
  - Tell me which projects in my resume to keep or remove to tailor it to the job
  - Generate a personalized cover letter based on the job description
- You do not need to remember any job description after you've processed it.

Here is my resume:
[Paste your resume here]
```

⬆️ This sets the context for the rest of our conversation **within this session**.

---

#### 🔹 Step 2: For each job description afterward

Just send messages like:

```
Here’s a new job description:

[Paste job description here]
```

And I will follow the rules you gave at the start (analyze the job, pick projects, generate a cover letter) — no need to repeat anything else.

---

### 🧠 Bonus: Shortcut for future use

If you’ll be doing this frequently, you can save your initial setup prompt and just paste it in every new session. Or, if you’re using the **ChatGPT custom instructions** feature (in ChatGPT Pro), you could paste your resume and rules into:

- “What would you like ChatGPT to know about you?”
    
- “How would you like ChatGPT to respond?”
    

Let me know if you'd like help writing that as a reusable template or setting it up programmatically.