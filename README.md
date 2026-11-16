## Reflection Questions

**1. Why is it dangerous to store passwords in plain text, and how does hashing prevent harm if the database is compromised?**
If passwords are stored in plain text, anyone who gets into the database can read every user's password immediately. Since many people reuse passwords, this can also compromise their accounts on other sites. Hashing with bcrypt turns each password into a one-way string that can't be reversed. Even if the database leaks, attackers only get hashes, and bcrypt's salting and slowness make cracking them very difficult.

**2. Why is returning the same error for "user not found" and "wrong password" better than specific messages?**
Specific messages tell an attacker which emails are registered in the system. They could then focus on guessing passwords only for real accounts, or use the list for phishing. Returning "Invalid credentials" in both cases gives away nothing about which part was wrong. This makes it harder to find valid accounts.

**3. What does the middleware do on every protected request, and why is it better than putting token-checking code in every route handler?**
The middleware reads the Authorization header, checks that it has a Bearer token, and verifies the token with the JWT secret. If the token is missing or invalid it returns 401, otherwise it saves the decoded user in `req.user` and lets the request continue. Writing this once and reusing it with `requireAuth` avoids repeated code and mistakes. If the checking logic ever needs to change, I only have to update one file.

**4. What is one thing that could still go wrong, and how would you address it?**
A token could be stolen, for example if the app runs over plain HTTP, since anyone who has the token can act as that user until it expires. In a real application I would serve the API over HTTPS only and keep token lifetimes short. I would also use refresh tokens that can be revoked, and store the JWT secret securely instead of in a plain file.
