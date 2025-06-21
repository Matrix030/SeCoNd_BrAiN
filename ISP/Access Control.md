Here's a detailed summary of **internet security and privacy** concepts from the presentation **CS392 6 – Access Control (Part 1)**:

---

## 🔐 **Core Concept: Access Control**

**Access control** determines:

- What users can do
    
- Which resources they can access
    
- What operations they can perform
    

### 🧱 Elements:

- **Subject**: Active entity (user/process)
    
- **Object**: Passive entity (file/resource)
    
- **Access**: Flow of data between subject and object
    

Access control enforces **confidentiality, integrity, availability (CIA)**.

---

## 🛡️ **Key Components**

1. **Identification**: Who you claim to be (e.g., username)
    
2. **Authentication**: Proof of identity (e.g., password, biometrics)
    
3. **Authorization**: What actions you're allowed to perform
    

These three together form the **access decision pipeline**.

---

## 🔐 **Access Control Models**

### 1. **Discretionary Access Control (DAC)**

- **Owner-controlled**: Resource owner sets access permissions.
    
- Common in Unix systems.
    

### 2. **Mandatory Access Control (MAC)**

- **Label-based security**: Users and resources have security levels.
    
- Used in high-security environments (e.g., military).
    

### 3. **Role-Based Access Control (RBAC)**

- **Roles define permissions**, and users are assigned roles.
    
- Easier to manage in large organizations with high staff turnover.
    

### 4. **Relation-Based Access Control (ReBAC)**

- Access is determined by relationships (e.g., “friends” on social media).
    

---

## 🧰 **Access Control Techniques**

### ✅ Access Control Matrix (ACM)

- Table defining what each subject can do to each object.
    
- Rows = subjects, columns = objects, entries = permissions
    
- Scales poorly—leads to other implementations like:
    

### ✅ Access Control Lists (ACLs)

- Each **object** has a list of which **subjects** have what permissions.
    
- Used in Unix, Windows, and database systems.
    

### ✅ Capabilities (covered later)

- Opposite of ACLs: permissions are stored with **subjects**.
    

---

## 🧱 **Operating System Mechanisms**

### 🖥️ **Unix**

- Files have owner, group, and “other” permissions: `rwxrwxrwx`
    
- **Setuid/Setgid/Sticky bits** for privilege escalation.
    
- Limitations:
    
    - Can't finely control access for more than 3 categories.
        
    - Vulnerable if SUID programs are exploited.
        

### 🪟 **Windows**

- ACLs are more flexible.
    
- Uses **tokens**, **security descriptors**, and **Access Control Entries (ACEs)**.
    
- **Explicit deny** takes precedence over allow.
    
- Complex but allows granular permission tuning.
    

### 📱 **Android**

- Each app is assigned a unique UID.
    
- Apps declare required permissions in a manifest.
    
- Sandboxing enforced by the OS.
    
- Runtime permission model since Android 6.0.
    

---

## ⚠️ **Security and Privacy Implications**

- **Unix**: Inflexible permissions can leak access unintentionally.
    
- **Windows**: Misconfigured ACLs or tokens can expose private data.
    
- **Android**: Outdated apps or poorly configured manifests can risk data leaks.
    
- **Web/Cloud**: RBAC + dynamic contexts (location, time, device) affect what users see or can do.
    

---

## 🧠 Summary Takeaways for Internet Security & Privacy

- Strong **access control** enforces **security and privacy** by design.
    
- **MAC** and **RBAC** are better than DAC for sensitive systems.
    
- OS-level mechanisms (ACLs, tokens, sandboxes) are your front-line defense.
    
- Beware of over-privileged apps/programs—**least privilege** is key.
    
- **Audit trails and deny-by-default** policies help prevent privacy breaches.
    

---

Let me know if you want this converted into Obsidian notes or a study cheat-sheet.