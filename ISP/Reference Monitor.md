Here’s a focused summary of the **internet security and privacy** content from the presentation **CS3923/6813 5 – PL Reference Monitors**:

---

## 🔐 **What Are Reference Monitors?**

A **reference monitor** is a security mechanism that enforces access control policies by mediating all access to objects (files, resources) from subjects (users/processes).

### Key Requirements:

- **Complete Mediation**: Every access must go through the monitor.
    
- **Tamper-Proof**: Cannot be bypassed or altered.
    
- **Verifiable**: Should be small and simple enough to verify correctness.
    

---

## 🧱 **Access Control Concepts**

- **Access Control** defines **what users can do**, which **resources** they can access, and what **operations** they can perform.
    

Example: Immigration checkpoint

- Layers of checks: Passport → Visa → Entry Decision
    
- Mirrors how layered access control works in systems
    

---

## 📑 **Reference Monitor Models**

### 1. **Capability Model**

- Permissions are attached to **subjects** (users/processes).
    
- Easier to **chain multiple monitors** and pass access securely.
    
- Example: Replacing a sensitive capability with a restricted one through a monitor.
    

### 2. **Access Control List (ACL) Model**

- Permissions are attached to **objects** (files/resources).
    
- Uses **system call interposition** (monitor sits between process and OS).
    
- Much **harder to implement correctly** due to:
    
    - Race conditions (TOCTTOU)
        
    - Complex path and symbolic link resolution
        
    - System call complexity and OS state replication
        

---

## ⚠️ **System Call Interposition Risks (ACL-Based)**

- Difficult to handle all edge cases (e.g., symlink races).
    
- Easy to break functionality or leave security holes.
    
- Not recommended for strong isolation/security enforcement—used mostly for debugging.
    

---

## 🔒 **Sandboxing via Programming Language VMs**

- Run untrusted code inside a virtualized environment with built-in checks (e.g., Java, JavaScript, Repy).
    
- Security is enforced at:
    
    - **Computation layer**: type checking, object creation
        
    - **Standard library layer**: restrict file/network access
        

#### Problems with traditional PL sandboxing:

- **Large TCB**: Any bug in the language VM can compromise everything.
    
- **Hard to isolate privileges**: Most traditional libraries assume full access.
    

---

## ✅ **Repy V2: Secure Execution Environment**

Repy (Restricted Python) used in Seattle Testbed provides:

- **Small TCB** (Tiny trusted core)
    
- **Externalized privileges**: Capabilities passed explicitly
    
- **Failsafe isolation**: Faults outside TCB do not compromise security
    

**Components:**

- Sandbox Kernel: Exposes only 32 finely scoped capabilities.
    
- Encapsulation Libraries: Prevent TOCTTOU bugs, enforce policy.
    
- Policy Libraries: For logging, access control, controlled communication.
    

---

## 🔁 **Real-World Vulnerabilities & Privacy Relevance**

Examples of Java bugs show:

- Even **signed** or **verified code** can be exploited if the platform’s security checks are weak.
    
- Reflection, serialization, and loader components are frequent attack vectors.
    

> Security bugs are often **minimally disclosed**—a major privacy risk.

---

## 🎯 Key Takeaways for Internet Security & Privacy

- Capability systems are more secure and modular for enforcing access control.
    
- Reference monitors must be carefully designed to avoid race conditions and system inconsistencies.
    
- Programming language sandboxes offer some isolation but are only as strong as their TCB.
    
- **Minimizing trust**, **explicit policy enforcement**, and **small, verifiable components** improve both security and privacy.
    
- **Vulnerabilities in language VMs** can silently compromise personal data and platform trust.
    

---

Let me know if you'd like this exported as an Obsidian-friendly note or flashcards.