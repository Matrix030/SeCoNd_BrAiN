- **Access control is only effective in trusted server-side code or server-less API, where the attacker cannot modify the access control check or metadata.**  
    → _Only enforce permissions on the backend, not in the browser where hackers can change things._
    
- **Except for public resources, deny by default.**  
    → _Block everything by default and only allow what’s truly needed._
    
- **Implement access control mechanisms once and re-use them throughout the application, including minimizing Cross-Origin Resource Sharing (CORS) usage.**  
    → _Write one clear rule system for access and use it everywhere. Don’t allow cross-site access unless absolutely needed._
    
- **Model access controls should enforce record ownership rather than accepting that the user can create, read, update, or delete any record.**  
    → _Users should only be able to see or change their own data, not someone else’s._
    
- **Unique application business limit requirements should be enforced by domain models.**  
    → _Put rules like “only 5 uploads per day” in the backend where they can't be bypassed._
    
- **Disable web server directory listing and ensure file metadata (e.g., .git) and backup files are not present within web roots.**  
    → _Don’t leave secret files or folders open on the website for anyone to see._
    
- **Log access control failures, alert admins when appropriate (e.g., repeated failures).**  
    → _Keep a record when someone tries to access something they shouldn’t, and alert admins if it happens often._
    
- **Rate limit API and controller access to minimize the harm from automated attack tooling.**  
    → _Slow down bots by limiting how often they can make requests._
    
- **Stateful session identifiers should be invalidated on the server after logout. Stateless JWT tokens should rather be short-lived so that the window of opportunity for an attacker is minimized. For longer lived JWTs it's highly recommended to follow the OAuth standards to revoke access.**  
    → _After logout, make sure login tokens stop working. If using long-lasting tokens, allow them to be revoked to block hackers._
