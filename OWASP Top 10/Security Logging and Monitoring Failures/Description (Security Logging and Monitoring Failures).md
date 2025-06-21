Returning to the OWASP Top 10 2021, this category is to help detect, escalate, and respond to active breaches. Without logging and monitoring, breaches cannot be detected. Insufficient logging, detection, monitoring, and active response occurs any time:

- Auditable events, such as logins, failed logins, and high-value transactions, are not logged.
    
- Warnings and errors generate no, inadequate, or unclear log messages.
    
- Logs of applications and APIs are not monitored for suspicious activity.
    
- Logs are only stored locally.
    
- Appropriate alerting thresholds and response escalation processes are not in place or effective.
    
- Penetration testing and scans by dynamic application security testing (DAST) tools (such as OWASP ZAP) do not trigger alerts.
    
- The application cannot detect, escalate, or alert for active attacks in real-time or near real-time.
    

You are vulnerable to information leakage by making logging and alerting events visible to a user or an attacker (see [A01:2021-Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)).