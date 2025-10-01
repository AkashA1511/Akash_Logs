# Host Header Injection / Open Redirect Vulnerability

Host Header Injection occurs when an attacker manipulates the `Host` header in HTTP requests to influence application behavior, potentially redirecting victims to malicious sites or performing phishing attacks.

---

## How It Works

An attacker intercepts a request and modifies the `Host` header.  
If the application does not properly validate the header, the attacker can control redirections or modify application behavior.

Example headers an attacker might use:
[Host: target.com@evil.com]
[X-Forwarded-Host: attackingsite]

They can also modify the `Referer` header or request method to bypass protections.

---

### Impact

- Phishing attacks
- Open redirect vulnerabilities
- Potential bypass of security controls

---

## Testing for Host Header Vulnerability

### Step-by-Step:
1. Identify a target application.
2. Intercept the request.
3. Modify the `Host` header.
4. Observe if the application response changes.

---

### Response Behavior:
| Response Code | Meaning                             |
|---------------|--------------------------------------|
| 2XX           | Successful request (200, 201...)    |
| 3XX           | Redirection (300, 301, 302...)      |
| 403, 404      | Forbidden or Not Found              |

Example target:  


GET /artists.php HTTP/1.1
[Host: www.testphp.vulnweb.com]

User-Agent: Mozilla/5.0
[X-Forwarded-Host: www.bing.com]


Other variations:
1. `Host: inflectra.com`  
   `X-Forwarded-Host: www.bing.com`

2. `Host: www.bing.com`  
   `X-Forwarded-Host: inflectra.com`

3. `Host: www.zivame.com@www.bing.com`

4. Modify `Referer`:  
   `Referer: https://www.bing.com/`

5. Changing request method from GET to POST

---

## Common Vulnerable Pages
- Signup page  
- Login page  
- Forgot password page  
- Logout page  
- Contact us page  
- Any page accepting host-dependent requests

---

### Real-World Examples
1. Change host for:  
[https://www.conbinijapan.com/]

When clicking "English" after modifying the Host header, the attacker site opens.

2. Reset password endpoint:  
[https://inflectra.com/reset-password?token=kjcnkjckjchkjsjkdhfkjdfhdskjfhds]

can be manipulated similarly.

---

### HackerOne Reports:
- [Report #698416](https://hackerone.com/reports/698416)  
- [Report #13286](https://hackerone.com/reports/13286)  
- [Report #158019](https://hackerone.com/reports/158019)

---

### References:
- [Medium Guide: Exploiting Host Header Injection](https://gupta-bless.medium.com/exploiting-host-header-injection-5554fef7e25)
- [OWASP: Host Header Injection](https://owasp.org/www-community/attacks/Host_Header_Injection)

---

### How to Fix:
- Validate and whitelist host headers against known safe domains.
- Avoid relying on `Host` header for critical logic.
- Use absolute URLs for redirects instead of header values.
- Implement strict input validation for headers.

