Secure installation processes should be implemented, including:

- A repeatable hardening process makes it fast and easy to deploy another environment that is appropriately locked down. Development, QA, and production environments should all be configured identically, with different credentials used in each environment. This process should be automated to minimize the effort required to set up a new secure environment.
    
- A minimal platform without any unnecessary features, components, documentation, and samples. Remove or do not install unused features and frameworks.
    
- A task to review and update the configurations appropriate to all security notes, updates, and patches as part of the patch management process (see [A06:2021-Vulnerable and Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/)). Review cloud storage permissions (e.g., S3 bucket permissions).
    
- A segmented application architecture provides effective and secure separation between components or tenants, with segmentation, containerization, or cloud security groups (ACLs).
    
- Sending security directives to clients, e.g., Security Headers.
    
- An automated process to verify the effectiveness of the configurations and settings in all environments.