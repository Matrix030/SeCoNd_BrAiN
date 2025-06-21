Here’s a detailed and focused summary of **internet security and privacy** concepts covered in the presentation **CS3923 2 – Security Design Principles**:

---

## 🔐 Core Security Design Principles

### ✅ **General System Design Principles** (affecting security)

1. **Open Design**
    
    - Security should not depend on secrecy of the design.
        
    - Get others to review the system (e.g., “Linus’s Law”).
        
2. **Sweeping Simplifications (KISS)**
    
    - Simple systems are easier to understand, test, and secure.
        
    - Complexity is the enemy of security.
        
3. **Design for Iteration**
    
    - Make the system adaptable to future threats or changes.
        
    - Emphasize modularity and abstraction.
        
4. **Least Astonishment**
    
    - The system should behave in ways that users expect.
        
    - Errors and unexpected behavior open up security risks.
        

---

## 🛡️ Principles of Secure System Design

### 1. **Minimizing Secrets**

- Keep secrets (like passwords, keys) few and easily changeable.
    
- Don’t rely on “security through obscurity.”
    

### 2. **Complete Mediation**

- Every access to a resource must be checked.
    
- Avoid shortcuts like reusing unchecked permissions (e.g., UNIX FD issues).
    

### 3. **Fail-Safe Defaults**

- Default access should be **denied**, not allowed.
    
- Example: Firewall default deny rules.
    

### 4. **Least Privilege**

- Each component should have **only** the permissions it needs.
    
- Helps limit damage from compromised parts.
    

### 5. **Simplicity of Mechanism (Economy)**

- Smaller, simpler systems are easier to verify and secure.
    
- Reduce Trusted Computing Base (TCB).
    

### 6. **Least Common Mechanism**

- Avoid sharing mechanisms across users or subsystems.
    
- Prevents unintended data leaks or interference.
    

---

## 📱 Real-World Privacy & Security Failures (What Could Possibly Go Wrong?)

- **Voting Registration Exploits**: Forms that allow deregistration using publicly leaked data (SSNs, DOBs).
    
- **Malicious USB Ports**: Charging stations as attack vectors (e.g., hidden AT commands).
    
- **Copiers storing sensitive data**: Internet-connected copiers storing scanned tax documents.
    
- **Smartphones shipped with exploitable bloatware**.
    

---

## 🧠 Password Security Insights

- Passwords are reused frequently, even after leaks (e.g., Yahoo, LinkedIn, Evernote).
    
- **20% of breached credentials** match Microsoft account logins due to reuse.
    
- Password managers help generate per-site strong passwords.
    
- Even password managers can be bypassed via browser autofill vulnerabilities.
    

### Good Practice:

- Use password managers (e.g., LastPass, 1Password).
    
- Apply methods like “correct horse battery staple” for passphrase creation.
    
- Don’t reuse passwords across sites.
    

---

## 🤔 Ethics & Legal Questions

- **White Hat Worms**: Is it ethical to patch vulnerabilities by breaking into systems?
    
- **FBI hacking Tor**: Does fighting crime justify violating user privacy?
    
- **Comcast injecting ads** into public Wi-Fi traffic—ethical or exploitative?
    
- **Revenge Porn, Swatting, Botnet Takedowns**: How legal measures impact online threats.
    

---

## ✅ Summary Takeaways for Internet Security & Privacy

- **Design matters**: Simplicity, openness, and least privilege reduce attack surfaces.
    
- **Usability and expectations** are vital—poor UX leads to user mistakes and privacy breaches.
    
- **Trust is earned** through design choices, not hidden mechanisms.
    
- **Ethics and legality** evolve with technology—question the impact of your solutions.
    
- **Passwords remain a major weak point**—handle them with care and strategy.
    

---

Let me know if you'd like this broken down into Obsidian-ready bullet points!