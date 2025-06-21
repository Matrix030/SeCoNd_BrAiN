Here is a focused summary of **internet security and privacy** concepts from **CS3923 7 – Access Control Part 2**:

---

## 🔐 **Capability Systems vs. ACLs**

### 🔑 Capabilities (Subject-based)

- An **unforgeable token** granting rights to objects.
    
- **Stored securely** (OS, language runtime, etc.).
    
- Easier for **delegation**: pass capability to another process.
    
- Harder for **revocation**: often needs indirection or capability rewriting.
    
- **Example use**: Kerberos tickets, Amoeba OS.
    

### 📋 Access Control Lists (Object-based)

- Each object stores a list of **who can do what**.
    
- Easier for **revocation**: just remove from the list.
    
- Used widely (e.g., Windows, Unix).
    
- **Harder to delegate** without impersonation.
    

> 🔁 **ACLs easier to manage for audit/compliance**; **capabilities better for distributed or fine-grained control.**

---

## 🛡️ **Reference Monitors**

### Key Requirements:

- **Complete mediation**
    
- **Tamper-proof**
    
- **Verifiable (small and simple)**
    

### ACL-style Reference Monitors:

- Use **system call interposition** (trap OS calls).
    
- Vulnerable to **TOCTTOU** (Time of Check to Time of Use) issues:
    
    - Symlinks, relative paths, argument replacement
        
    - Multi-threaded/shared memory races
        
- Difficult to chain and maintain.
    

### Capability-style Reference Monitors:

- Easier to enforce.
    
- Can be **chained**: wrap capability with checks and pass forward.
    
- Safer and more modular.
    
- Example: Amoeba OS where each object is accessed via capability tokens.
    

---

## 🧱 Real-World Vulnerability Example: **Kubernetes Subpath Attack**

- Kubernetes allowed mounting user-defined subpaths in containers.
    
- Attack: Create a symlink `data1 → /`, trick system to mount host root FS.
    
- Fix involved:
    
    - Validating symlinks safely with `O_NOFOLLOW`
        
    - Bind mounting to a **validated safe path**
        
    - Enforcing **same filesystem checks**
        

> ⚠️ Real-world access control flaws can **expose entire host filesystems** in containerized environments.

---

## 🌐 **Browser as an OS (Chrome Security Architecture)**

### Web browsers protect **privacy and security** through:

- **Origins** as security principals instead of user IDs
    
- **Sandboxing** of rendering processes
    
- **Isolation** of plugins, JS execution, localStorage, etc.
    

### Key Browser Security Concepts:

- **Frame isolation**: Who can read/write content or navigate others?
    
- **Cookie access**: Which scripts can access site cookies?
    

### Threats mitigated:

- Malware: cannot write to file system.
    
- File Theft: sandboxing limits access.
    
- Keyloggers: isolated processes.
    

---

## 🔄 **Information Flow Control (IFC)**

### Problem with traditional access control:

- Protects data **at source**, but not **as it moves** through system.
    

### Solution:

- **Tag sensitive data** (e.g., credit card numbers).
    
- Track how tagged data flows through variables/functions.
    
- **Block or declassify** data before it reaches outputs (e.g., network).
    

### Concepts:

- **Non-interference**: confidential data shouldn't affect observable outputs.
    
- **Declassification**: controlled, intentional release of sensitive data.
    
- **Principals**: define who can read/write data.
    
- **Labels**: combine policies on data for confidentiality & integrity.
    

---

## ✅ Key Takeaways for Internet Security & Privacy

- **Capability systems** allow secure, flexible, fine-grained access but are harder to revoke.
    
- **ACLs** are intuitive and manageable but complex to implement securely at runtime.
    
- **Reference monitors** need careful implementation—race conditions and incomplete mediation are real risks.
    
- **IFC** tracks how data moves—useful when protecting **user privacy and preventing data leaks**.
    
- Modern systems (e.g., **Chrome, Kubernetes**) require layered, composable security models.
    

---

Let me know if you want this summarized into Obsidian bullet points or made into flashcards.