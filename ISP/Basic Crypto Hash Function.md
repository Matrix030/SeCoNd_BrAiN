Here's a detailed summary focused **only** on **internet security and privacy** aspects from the presentation **CS3923 – Basic Crypto / Hash Functions**:

---

## 🔐 Cryptography Overview

- **Purpose**: Securing data via confidentiality, integrity, authentication, and non-repudiation.
    
- Not a complete security solution, but a **core component** of information security.
    
- Based on keeping **keys secret**, not the algorithm (**Kerckhoffs’ Principle**).
    

---

## 🔑 Core Cryptographic Models

### 1. **Symmetric Key Cryptography**

- Same key for encryption and decryption.
    
- Fast, but **key distribution** is hard (requires secure channel).
    
- Examples:
    
    - **AES (Advanced Encryption Standard)**: Current standard, 128/192/256-bit keys.
        
    - **Triple DES**: Older standard, applies DES three times.
        
    - **One-Time Pad**: Unbreakable if key is random, same length, and never reused.
        

### 2. **Asymmetric Key Cryptography**

- Public key for encryption, private key for decryption.
    
- Easier key distribution, but **slower**.
    
- Algorithms:
    
    - **RSA**: Based on factoring large numbers.
        
    - **Diffie-Hellman**: Secure key exchange, but no authentication.
        
    - **Elliptic Curve Cryptography (ECC)**: Smaller keys, same security.
        
    - **EdDSA**: Signature scheme with better performance and security.
        

> 🔐 Asymmetric crypto is essential for **internet protocols** like HTTPS, SSH, PGP, etc.

---

## 🔐 Hash Functions & Message Integrity

### Cryptographic Hash Requirements:

- Input: any size → Output: fixed size (e.g., 256 bits)
    
- Fast and deterministic
    
- One-way (can’t reverse the hash)
    
- Collision-resistant:
    
    - **Weak**: Hard to find another input that hashes to the same.
        
    - **Strong**: Hard to find _any_ two inputs with the same hash.
        

### Algorithms:

- **SHA-256 (recommended)**
    
- **MD5**, **SHA-1** (broken, avoid)
    

### Attacks:

- **Birthday attack**: ~2ⁿ⁄² attempts needed to find a collision (e.g., 2⁸⁰ for SHA-1).
    

---

## 📬 Digital Signatures (Authentication + Integrity)

- Uses **asymmetric crypto** for:
    
    - Ensuring sender authenticity
        
    - Integrity of message
        
    - Non-repudiation (can’t deny having sent it)
        

### Example (RSA-based):

1. Alice signs a hash of message `h(M)` with her **private key**.
    
2. Bob verifies it using Alice’s **public key**.
    
3. Ensures message hasn’t been tampered and was really from Alice.
    

### HMAC (Keyed-Hash MAC)

- Combines **symmetric keys** + **hash functions**.
    
- Used for **fast, efficient authentication** (no non-repudiation).
    
- Immune to length extension attacks if implemented as HMAC (not naive `H(key || msg)`).
    

---

## 🎲 Cryptographically Secure Random Number Generation (CSPRNG)

- Needed for generating:
    
    - Keys
        
    - Nonces
        
    - Initialization vectors
        
- Must be:
    
    - **Unpredictable**
        
    - **Indistinguishable from true random**
        
- Examples:
    
    - Linux `/dev/urandom`
        
    - Hardware-based RNGs
        
    - Lava lamps (used by Cloudflare for entropy)
        

> 🔐 If your randomness is weak, **everything else can break.**

---

## 📡 Quantum Cryptography (Future Privacy)

- **Quantum Key Distribution (QKD)**:
    
    - Unbreakable due to physics (measuring a photon changes it).
        
    - BB84 protocol: Discard intercepted keys.
        
    - Used for **secure key exchange**, even on **untrusted networks**.
        

---

## 🔒 Homomorphic Encryption (Cloud Privacy)

- Enables computation **on encrypted data** without decryption.
    
- Application:
    
    - Privacy-preserving search
        
    - Outsourcing computation to cloud providers securely
        

> 🔐 Helps maintain **data confidentiality** even during processing.

---

## 🔮 Post-Quantum Cryptography

- Traditional algorithms (RSA, ECC) break with quantum computers.
    
- New directions:
    
    - **Lattice-based**
        
    - **Multivariate polynomial**
        
    - **Isogeny-based**
        
- Goal: Remain secure even against quantum attackers.
    

---

## 🕵️‍♂️ Private Information Retrieval (PIR)

- Problem: Download data (e.g., news, patches) **without revealing what you're downloading**.
    
- Use multiple mirrors, encryption, and obfuscation.
    
- Protects user **privacy from surveillance** or tracking.
    

---

## ✅ Takeaways for Internet Security & Privacy

|Feature|Use in Internet Privacy|
|---|---|
|**Symmetric Crypto**|Fast data encryption (e.g., AES for VPNs)|
|**Asymmetric Crypto**|Secure key exchange (e.g., HTTPS/SSL)|
|**Hash Functions**|Password storage, data integrity|
|**Digital Signatures**|Authenticated, tamper-proof messages|
|**CSPRNG**|Safe key/nonces generation|
|**Quantum Crypto**|Physically unbreakable key exchange|
|**Homomorphic Encryption**|Secure cloud computation|
|**Post-Quantum Algorithms**|Future-proof privacy|
|**PIR**|Protects access patterns online|

---

Let me know if you'd like a condensed version or note-ready version for Obsidian.