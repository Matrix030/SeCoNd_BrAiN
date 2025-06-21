- Sanitize and validate all client-supplied input data
    
- Enforce the URL schema, port, and destination with a positive allow list
    
- Do not send raw responses to clients
    
- Disable HTTP redirections
    
- Be aware of the URL consistency to avoid attacks such as DNS rebinding and “time of check, time of use” (TOCTOU) race conditions
    

Do not mitigate SSRF via the use of a deny list or regular expression. Attackers have payload lists, tools, and skills to bypass deny lists.