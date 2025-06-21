Here’s a focused summary of **internet security and privacy** topics from **CS3923 – PKI Theory**:

---

## 🔐 **What is a PKI (Public Key Infrastructure)?**

- A **PKI** solves problems with **public-key cryptography**:
    
    - How to **associate identities** with keys?
        
    - How to **distribute** and **update keys**?
        
    - How to **trust** public keys?
        
- Enables **digital signatures**, **confidentiality**, **authentication**, and **non-repudiation** online.
    

---

## 📄 **Digital Certificates**

- A **digital certificate** binds a **public key to an identity**, signed by a **Certificate Authority (CA)**.
    
- Follows standard format: **X.509**
    
- Example fields:
    
    - Subject: CN=Alice, O=NYU
        
    - Issuer: CN=CA, O=NYU
        
    - Public Key, Signature, Validity Dates
        

> ✅ Ensures authenticity and integrity of public keys on the internet (used in HTTPS, email, etc.)

---

## 🏛️ **PKI Trust Models**

### 1. **Hierarchical Model**

- Root CA → Intermediate CAs → End-user certs
    
- Easy to implement and audit
    
- Common in browsers and operating systems
    

### 2. **Cross-Certification**

- Two CAs certify each other
    
- Used to link independent infrastructures
    

### 3. **Bridge CA**

- A central CA links multiple independent PKIs
    
- Used by governments (e.g., Federal Bridge CA)
    

> 🔗 Trust is established via **trust chains** up to a trusted root (trust anchor).

---

## 🔐 **Certificate Authorities (CAs)**

- Responsible for:
    
    - Issuing and revoking certificates
        
    - Maintaining CRLs/OCSP servers
        
    - Keeping private keys secure
        

> 🔥 If a CA is hacked, **fraudulent certificates** can be trusted by all systems (e.g., DigiNotar breach).

---

## 🔁 **Revocation Mechanisms**

### 1. **Certificate Revocation List (CRL)**

- List of revoked certs signed by CA
    
- Can grow large; delays in publishing are risky
    

### 2. **Online Certificate Status Protocol (OCSP)**

- Real-time status check (Good, Revoked, Unknown)
    
- More responsive, but subject to DoS or stale URLs
    

---

## 📜 **PKI Documents:**

- **Certification Policy (CP)**: What the CA does
    
- **Certification Practice Statement (CPS)**: How it operates
    
- Important for **audits, legal compliance**, and **trust decisions**
    

---

## 🔍 **Certificate Transparency (CT)**

- Solution to CA misbehavior (e.g., DigiNotar)
    
- Public, **append-only logs** of issued certificates
    
- Uses **Merkle Trees** to ensure log integrity
    
- Browsers (Chrome, Safari) require certs to be logged in CT logs
    

> 👁️‍🗨️ Domain owners can monitor CT logs for **unauthorized certs** issued for their domains.

---

## 📦 PKI in Practice

- Used in:
    
    - **SSL/TLS** (HTTPS)
        
    - **Secure Email** (S/MIME)
        
    - **VPNs** (IPsec)
        
    - **Signed software**, **XML**, **wireless auth**
        
- Supported by:
    
    - Browsers (Chrome, Firefox, Safari…)
        
    - OSs (Windows, macOS, Linux)
        
    - Crypto libraries (OpenSSL, NSS, GnuTLS)
        

---

## 🛡️ Security & Privacy Enhancing PKI Features

|Feature|Relevance|
|---|---|
|**Digital Signatures**|Authenticity + non-repudiation|
|**Public/Private Keys**|No pre-shared secrets over internet|
|**Certificate Validation**|Protects against spoofed sites|
|**Revocation**|Prevents use of compromised keys|
|**CT Logs**|Transparency and auditability|
|**Time Stamping**|Proof of data existence before a time|
|**Smart Cards / TPMs**|Hardware-protected keys, harder to steal|

---

## 🚨 Security Failures in PKI

### DigiNotar Hack:

- Weak Windows domain password allowed attacker to issue **531 fake certs** (e.g., Google, Facebook, CIA).
    
- Certs were trusted by **all major browsers** due to implicit trust in CA.
    

> ⚠️ Highlights the **centralization risk** of PKI.

---

## ✅ Summary for Internet Security & Privacy

- **PKI is essential** for secure communication over the internet.
    
- Weaknesses in PKI (bad CA practices, slow revocation, poor transparency) **directly affect privacy**.
    
- Modern efforts like **Certificate Transparency** aim to reduce **blind trust** in centralized authorities.
    
- Best practices include **monitoring, minimal trust, and cryptographic hygiene** (e.g., revocation, secure key storage).
    

---

Let me know if you’d like this compiled into a full Obsidian-compatible note.