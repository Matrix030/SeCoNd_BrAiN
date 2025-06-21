**CORS** stands for **Cross-Origin Resource Sharing**.

### 🧠 In Simple Terms:

CORS is a **browser security feature** that controls how web pages can **request resources** (like data or APIs) **from another domain** (a different "origin").

### 📍 What is an Origin?

An **origin** is defined by:
- **Scheme** (http or https)
- **Domain**
- **Port**

For example:

- `https://example.com:443` is a different origin than `http://example.com:80`

### 🔒 Why is CORS needed?

By default, browsers **block** cross-origin HTTP requests initiated from scripts, for **security**. This prevents malicious websites from accessing sensitive data on other domains using your browser.

---

### 🛂 How does CORS work?

When a frontend JavaScript code tries to fetch data from another origin:

1. The browser sends an **HTTP request** with a special header like: