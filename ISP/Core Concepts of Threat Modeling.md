Here’s a focused and detailed overview of the **internet security and privacy** content from the presentation **CS3923 3 – Threat Modeling**:

---

## 🔐 Core Concepts of Threat Modeling

### 💡 Why Threat Modeling?

- Identifies **design flaws** and **vulnerabilities** early.
    
- Helps prioritize **security efforts** and mitigation strategies.
    
- Makes you understand your **system, assets, attackers, and risks** better.
    
- Affects all phases of the **security lifecycle**, especially **policy, operation, and maintenance**.
    

---

## 🧱 Definitions You Must Know

- **Threat**: A potential event that could harm assets (e.g., attacker motivation).
    
- **Vulnerability**: A flaw that can be exploited to trigger a threat.
    
- **Attack**: An action exploiting a vulnerability.
    
- **Risk**: The expected loss if a threat occurs (think: impact × likelihood).
    

---

## 🧠 Structured Threat Models

### 1. **STRIDE** (Security Threats Framework by Microsoft)

|Type|Description|
|---|---|
|**Spoofing**|Faking identity to access data|
|**Tampering**|Modifying data illegally|
|**Repudiation**|Denying actions without traceability|
|**Information Disclosure**|Leaking confidential data|
|**Denial of Service**|Blocking access to resources|
|**Elevation of Privilege**|Gaining more permissions than authorized|

### 2. **LINDDUN** (Privacy Threat Framework)

|Type|Description|
|---|---|
|**Linkability**|Relating two actions to one user|
|**Identifiability**|Discovering user identity|
|**Non-repudiation**|User cannot deny their action|
|**Detectability**|Detecting user presence|
|**Disclosure**|Learning private data content|
|**Unawareness**|User unaware of data use|
|**Non-compliance**|Breaking data handling laws (like GDPR)|

---

## 📈 Risk Management Strategy

### Risk Handling Approaches:

- **Accept**: Live with it if risk is low or mitigation is too costly.
    
- **Transfer**: Shift risk via insurance or third parties.
    
- **Remove**: Eliminate risky features or components.
    
- **Mitigate**: Apply controls to reduce the threat.
    

### Cost-Effectiveness Metrics (Quantitative Approach):

- **SLE** (Single Loss Expectancy) = Cost of one event
    
- **ARO** (Annual Rate of Occurrence)
    
- **ALE** (Annual Loss Expectancy) = SLE × ARO
    
- **ACS** (Annual Cost of Safeguard)
    
- **ANB** (Annual Net Benefit) = ALE(before) - ALE(after) - ACS
    

> A safeguard is cost-effective if **ANB > 0**

---

## 🌲 Attack Trees

- Visual tool to model attacker goals and methods.
    
- **AND nodes** = all child steps needed.
    
- **OR nodes** = any child step is sufficient.
    
- Helps identify cheapest, easiest, or stealthiest attack paths.
    
- Attributes like **cost, difficulty, legality, special equipment** can be used to prioritize risks.
    

---

## 🔍 Privacy and Tracking: Real-World Focus

### Assignment Example: Online Tracking with Firefox Lightbeam

- Visualizes **third-party trackers** across websites.
    
- Reveals unexpected data sharing links between companies.
    
- Illustrates how **privacy is compromised** even during passive browsing.
    

### Key Takeaway:

Modern web services are full of **unseen linkages** that violate privacy unless controlled.

---

## ✅ Threat Modeling Summary

1. Identify **assets** (what you’re protecting).
    
2. Identify **threats** (how they can be attacked).
    
3. Perform **risk assessment** (how bad and how likely?).
    
4. Do **risk management** (accept, remove, transfer, or mitigate).
    

---

Let me know if you'd like this exported in Obsidian format.