- Segment remote resource access functionality in separate networks to reduce the impact of SSRF
    
- Enforce “deny by default” firewall policies or network access control rules to block all but essential intranet traffic.  
    _Hints:_  
    ~ Establish an ownership and a lifecycle for firewall rules based on applications.  
    ~ Log all accepted _and_ blocked network flows on firewalls (see [A09:2021-Security Logging and Monitoring Failures](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/)).