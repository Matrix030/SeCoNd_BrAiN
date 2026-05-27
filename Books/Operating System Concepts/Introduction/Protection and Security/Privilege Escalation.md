---
tags: [book, os, operating-systems, book-os-concepts, security]
---

# Privilege Escalation

1) In the course of normal use of a system, the user ID and group ID for a user are sufficient. However, a user sometimes needs to **escalate privileges** to gain extra permissions for an activity. The user may need access to a device that is restricted, for example.
2) Operating systems provide various methods to allow privilege escalation.
3) On UNIX, the **setuid** attribute on a program causes that program to run with the user ID of the owner of the file, rather than the current user's ID. The process runs with this **effective UID** until it turns off the extra privileges or terminates.

## Related

- [[> Protection and Security]] — back to the sub-topic MOC
- [[User and Group Identifiers]] — the user and group IDs that privilege escalation overrides
- [[Protection]] — protection mechanisms that privilege escalation interacts with
