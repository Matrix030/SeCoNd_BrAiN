Access control enforces policy such that users cannot act outside of their intended permissions. Failures typically lead to unauthorized information disclosure, modification, or destruction of all data or performing a business function outside the user's limits. Common access control vulnerabilities include:
- Violation of the principle of least privilege or deny by default, where access should only be granted for particular capabilities, roles, or users, but is available to anyone.

- Bypassing access control checks by modifying the URL (parameter tampering or force browsing), internal application state, or the HTML page, or by using an attack tool modifying API requests.

- Permitting viewing or editing someone else's account, by providing its unique identifier (insecure direct object references)

- Accessing API with missing access controls for POST, PUT and DELETE.

- Elevation of privilege. Acting as a user without being logged in or acting as an admin when logged in as a user.

- Metadata manipulation, such as replaying or tampering with a JSON Web Token (JWT) access control token, or a cookie or hidden field manipulated to elevate privileges or abusing JWT invalidation.

- [[CORS]] misconfiguration allows API access from unauthorized/untrusted origins.

- Force browsing to authenticated pages as an unauthenticated user or to privileged pages as a standard user. **Forced browsing** is when an attacker manually accesses restricted URLs (like admin or user dashboards) without proper authorization — either as an unauthenticated user or as a low-privileged user. It exploits missing or weak access control checks on the server.
