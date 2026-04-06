---
tags: [security, owasp, web-security, moc]
aliases: ["OWASP TOP 10", "OWASP Top 10"]
---

# OWASP Top 10 — Map of Content

The OWASP Top 10 is the industry-standard awareness document for web application security. It lists the most critical security risks, ranked by prevalence and impact.

> [!warning] Why this matters
> Most real-world breaches exploit one or more of these 10 categories. Knowing them makes you a better developer, not just a better security engineer.

---

## The 10 Risks (2021 Edition)

| # | Risk | Notes |
|---|------|-------|
| A01 | [[> Broken Access Control]] | Most common — users doing things they shouldn't |
| A02 | [[Cryptographic Failures]] | Weak/missing encryption exposing sensitive data |
| A03 | [[> Injection]] | SQL, command, LDAP injection |
| A04 | [[> Insecure Design]] | Flawed architecture, not just bad code |
| A05 | [[> Security Misconfiguration]] | Default creds, open cloud storage, verbose errors |
| A06 | [[> Vulnerable and Outdated Components]] | Unpatched deps with known CVEs |
| A07 | [[> Identification and Authentication Failures]] | Broken login, session management |
| A08 | [[> Software and Data Integrity Failures]] | CI/CD tampering, insecure deserialization |
| A09 | [[> Security Logging and Monitoring Failures]] | Can't detect or respond to breaches |
| A10 | [[> Server-Side Request Forgery]] | Server makes requests on behalf of attacker |

---

> [!tip] Study approach
> For each risk, understand: **What is it? → How does it happen? → How do you prevent it?**
> Each note follows that structure.
