---
tags: [book, os, operating-systems, book-os-concepts, security]
---

# User and Group Identifiers

1) [[Protection]] and [[Security|security]] require the system to be able to distinguish among all its users. Most operating systems maintain a list of user names and associated **user identifiers (user IDs)**. In Windows Vista parlance, this is a **security ID (SID)**.
2) These numerical IDs are unique, one per user. When a user logs in to the system, the authentication stage determines the appropriate user ID for the user. That user ID is associated with all of the user's processes and threads. When an ID needs to be user readable, it is translated back to the user name via the user name list.
3) In some circumstances, we wish to distinguish among sets of users rather than individual users. For example, the owner of a file on a UNIX system may be allowed to issue all operations on that file, whereas a selected set of users may only be allowed to read the file.
4) To accomplish this, we need to define a group name and the set of users belonging to that group. Group functionality can be implemented as a system-wide list of group names and **group identifiers**.
5) A user can be in one or more groups, depending on operating-system design decisions. The user's group IDs are also included in every associated process and thread.

## Related

- [[> Protection and Security]] — back to the sub-topic MOC
- [[Protection]] — protection mechanisms that rely on user identification
- [[Privilege Escalation]] — how users gain permissions beyond their assigned ID
