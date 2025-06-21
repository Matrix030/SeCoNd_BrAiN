Here’s a focused summary of **internet security and privacy** content from **CS3923 – Privacy and Key Management (Part 2)**:

---

## 🔑 **Key Management Overview**

- **Key distribution** is critical for secure systems.
    
    - If an attacker gets the key, they break the system.
        

### Common Techniques:

- **Physical delivery**: e.g., secure couriers for military/diplomatic use.
    
- **Trusted third party**:
    
    - **Authentication Key Server**: Like Kerberos (uses symmetric keys).
        
    - **Public Notary/PKI**: Uses asymmetric cryptography, as in SSL/TLS.
        

---

## 🛡️ **Kerberos: Centralized Authentication**

- Developed at MIT for **distributed network authentication**.
    
- Based on the **Needham-Schroeder** protocol.
    

### Key Features:

- Central **Key Distribution Center (KDC)** stores secret keys for all users and services.
    
- Issues **non-corruptible tickets** for authentication.
    
- Uses **timestamps and lifetimes** to prevent:
    
    - **Replay attacks**
        
    - **Man-in-the-middle**
        
    - **Eavesdropping**
        
    - **Impersonation**
        

### Security Implications:

- Relies heavily on **time synchronization** (usually via NTP).
    
- **Single point of failure**: if KDC is down, no one can authenticate.
    
- Secure **initial login** with `kinit`; uses ticket-granting ticket (TGT) system.
    

---

## 🧅 **Tor (The Onion Router)** – Network Privacy

### Purpose:

- Designed to protect **online anonymity** by routing traffic through multiple relays.
    

### Threat Model:

- Attacker may watch your ISP or be the service provider (Bob).
    
- Tor ensures neither knows both who you are **and** what you access.
    

### Key Concepts:

- **Onion routing**: Encrypt data in layers → route through 3+ relays.
    
    - Entry relay sees you, not destination.
        
    - Exit relay sees destination, not who you are.
        
- **Directory servers** help clients find relays.
    

### Privacy Benefits:

- Used by: journalists, law enforcement, military, activists, everyday users.
    
- Shields metadata (IP, location) even if content is encrypted separately (e.g., HTTPS).
    
- **Decentralized trust**: No single relay knows the full picture.
    

### Risks:

- Exit nodes can sniff unencrypted traffic (e.g., HTTP, not HTTPS).
    
- Browser fingerprinting and plugins (JavaScript, Flash, PDFs) can deanonymize users.
    
- Performance challenges: congestion, limited relays, high variability.
    

---

## 💥 **DDoS (Distributed Denial-of-Service) Attacks**

- Goal: Disrupt service availability using **botnets** or reflection/amplification.
    

### Why DDoS Matters for Privacy & Security:

- **Targets infrastructure**: affects availability, one of the CIA triad.
    
- **Used to suppress speech**, take down privacy tools, or target political groups.
    

### Types of DDoS:

1. **Smurf Attack**: ICMP broadcast → overwhelms target.
    
2. **SYN Flood**: Half-open TCP connections exhaust server resources.
    
3. **Reflection SYN**: Spoofed IP + large number of responses.
    
4. **Ping of Death**: Oversized fragmented packets crash system.
    
5. **Slowloris**: Keeps HTTP connections open to exhaust web server threads.
    
6. **DNS Amplification**: Small query → massive DNS response to target.
    

### Defenses:

- Filtering spoofed packets
    
- Rate-limiting
    
- Firewalls and load balancers
    
- SYN cookies, ingress filtering, and stricter protocol handling
    

---

## ✅ Summary: Key Points for Internet Security & Privacy

|Topic|Security Relevance|
|---|---|
|**Kerberos**|Centralized, time-based ticket authentication to avoid impersonation and MITM attacks|
|**Tor**|Network anonymity through multi-hop encryption; protects users from surveillance and traffic analysis|
|**Key Management**|Foundation for trust and encryption in all secure systems; relies on PKI, Kerberos, etc.|
|**DDoS Attacks**|Threaten **availability**; must be mitigated to preserve access to privacy-preserving services|
|**Privacy Tools**|Require **anonymity at scale**, strong key distribution, and secure software setups|

---

Let me know if you'd like a final combined note or a condensed flashcard-style version for review.