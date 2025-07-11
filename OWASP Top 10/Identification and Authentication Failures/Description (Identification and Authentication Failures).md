Confirmation of the user's identity, authentication, and session management is critical to protect against authentication-related attacks. There may be authentication weaknesses if the application:

- Permits automated attacks such as credential stuffing, where the attacker has a list of valid usernames and passwords.
    
- Permits brute force or other automated attacks.
    
- Permits default, weak, or well-known passwords, such as "Password1" or "admin/admin".
    
- Uses weak or ineffective credential recovery and forgot-password processes, such as "knowledge-based answers," which cannot be made safe.
    
- Uses plain text, encrypted, or weakly hashed passwords data stores (see [A02:2021-Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)).
    
- Has missing or ineffective multi-factor authentication.
    
- Exposes session identifier in the URL.
    
- Reuse session identifier after successful login.
    
- Does not correctly invalidate Session IDs. User sessions or authentication tokens (mainly single sign-on (SSO) tokens) aren't properly invalidated during logout or a period of inactivity.