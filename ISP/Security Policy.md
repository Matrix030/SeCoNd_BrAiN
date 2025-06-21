Here’s a focused overview of the **internet security and privacy** content from the **CS3923 4 – Security Policy** presentation:

---

## 🔐 **What is a Security Policy?**

- A **security policy** is a set of rules defining **what is allowed and what isn’t**.
    
- It can be **human-readable** (e.g., company policies) or **machine-enforceable** (e.g., firewall rules).
    
- A system is **secure** if it starts in an authorized state and can’t reach an unauthorized one.
    

---

## 🧱 **Security Goals (CIA Triad)**

|Goal|Definition|
|---|---|
|**Confidentiality**|No unauthorized access to information.|
|**Integrity**|No unauthorized modification of data.|
|**Availability**|Authorized users should have reliable access to resources when needed.|

---

## 🛡️ **Examples of Real-World Policies Affecting Privacy & Security**

### 🔍 Windows 10 Privacy Concerns

- Syncs all local + cloud data to Microsoft servers by default.
    
- Tracks websites open in your browser.
    
- Collects crash reports with **personally identifiable info (PII)**.
    
- Shares your **WiFi password with contacts** by default.
    

> **Privacy takeaway**: User consent and control are often unclear or buried in defaults.

---

## 🏛️ **Security Models and Policies**

### 1. **Bell-LaPadula (BLP) Model** – Focuses on **Confidentiality**

- **No read up** (can’t read data at higher security level).
    
- **No write down** (can’t leak higher-level data to lower levels).
    

### 2. **Biba Model** – Focuses on **Integrity**

- **No read down** (don’t trust low-integrity data).
    
- **No write up** (can’t corrupt high-integrity data).
    

### 3. **Clark-Wilson Model** – Enforces **well-formed transactions and separation of duty**.

- Tracks which users can run which verified programs on which data.
    

### 4. **Chinese Wall Model** – Prevents **conflict of interest**.

- If you access data from one company, you're blocked from accessing data from its competitors.
    

---

## 🧠 **Policy vs Mechanism**

- **Policy**: What is allowed/prohibited (rules).
    
- **Mechanism**: How it is enforced (e.g., ACLs, firewalls, encryption).
    

---

## 🧬 **Examples from NYU CS Department Security Policy**

|Measure|Security Property|
|---|---|
|Throttle login attempts|Availability|
|Audit all login attempts|Non-repudiation|
|Disable default passwords|Access Control|
|Encrypt full disk + files|Confidentiality|
|Delete unused files securely|Privacy, Confidentiality|
|Separate audit logs from main system|Integrity|

---

## 🧩 **Inclusive vs Exclusive Policies**

|Type|Description|
|---|---|
|**Inclusive**|States what is **allowed**, and all else is **prohibited** (more secure).|
|**Exclusive**|States what is **prohibited**, everything else is **implicitly allowed**.|

---

## 🔐 **How Policies Affect Privacy**

- **Access Control**: Who gets to see or edit personal info.
    
- **Audit Logs**: Keep track of access – who did what and when.
    
- **Encryption**: Protects sensitive info during storage or transmission.
    
- **Data Retention & Deletion**: Reduces exposure after data is no longer needed.
    

---

## 🔍 Takeaway for Internet Security & Privacy

- Policies must clearly define **who can access what, under what conditions**.
    
- **Defaults matter** – insecure defaults (like auto-sharing WiFi passwords) lead to privacy risks.
    
- **Auditing and enforcement** are just as important as writing the policy.
    
- Security policies must evolve with **threats, technology, and legal standards**.
    

---

Let me know if you want an Obsidian-format export or want this broken into a flashcard-style summary.