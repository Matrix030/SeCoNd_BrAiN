Do the following, at a minimum, and consult the references:

- Classify data processed, stored, or transmitted by an application. Identify which data is sensitive according to privacy laws, regulatory requirements, or business needs.

- Don't store sensitive data unnecessarily. Discard it as soon as possible or use PCI DSS compliant tokenization or even truncation. Data that is not retained cannot be stolen.
- → _Only keep sensitive info (like credit card numbers or passwords) if absolutely needed. If you can, delete it quickly or replace it with a secure token or partial info. Hackers can’t steal what doesn’t exist._

- Make sure to encrypt all sensitive data at rest.

- Ensure up-to-date and strong standard algorithms, protocols, and keys are in place; use proper key management.

- Encrypt all data in transit with secure protocols such as TLS with forward secrecy (FS) ciphers, cipher prioritization by the server, and secure parameters. Enforce encryption using directives like HTTP Strict Transport Security (HSTS).

- Disable caching for response that contain sensitive data.

- Apply required security controls as per the data classification.

- Do not use legacy protocols such as FTP and SMTP for transporting sensitive data.

- Store passwords using strong adaptive and salted hashing functions with a work factor (delay factor), such as Argon2, scrypt, bcrypt or PBKDF2.

- Initialization vectors must be chosen appropriate for the mode of operation. For many modes, this means using a CSPRNG (cryptographically secure pseudo random number generator). For modes that require a nonce, then the initialization vector (IV) does not need a CSPRNG. In all cases, the IV should never be used twice for a fixed key.
- → _The IV (a helper value for encryption) must be random or unique, depending on the encryption method. Use a secure random generator unless the method only needs a unique number (nonce). But no matter what, never reuse the same IV with the same key — it can break the encryption._

- Always use authenticated encryption instead of just encryption.

- Keys should be generated cryptographically randomly and stored in memory as byte arrays. If a password is used, then it must be converted to a key via an appropriate password base key derivation function.

- Ensure that cryptographic randomness is used where appropriate, and that it has not been seeded in a predictable way or with low entropy. Most modern APIs do not require the developer to seed the CSPRNG to get security.

- Avoid deprecated cryptographic functions and padding schemes, such as MD5, SHA1, PKCS number 1 v1.5 .

- Verify independently the effectiveness of configuration and settings.
