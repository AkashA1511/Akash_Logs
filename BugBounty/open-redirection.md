# Open Redirect Vulnerability

Open Redirect occurs when a web application allows attackers to redirect users to arbitrary external URLs without proper validation.  
This can be exploited for phishing, malware distribution, and bypassing security checks.

---

## How It Works

Example:
[https://www.yammer.com/contactus=success]

[https://www.yammer.com/contactus=https://evil.com]


Here, an attacker can redirect victims to any URL when a vulnerable parameter accepts the "`=`" symbol.

---

### Common Parameters to Test for Open Redirect

Attackers look for URL parameters that control redirection, such as:

- `URI=`
- `redirect=`
- `url=`
- `xyz=`
- `test=`
- `anything=`
- `contactus=`

💡 Using tools like **waybackurls**, attackers can find many such parameters in the target site.

---

### Attack Example

#### Simple Redirect Payload:
[https://www.yammer.com/redirect?=https://evil.com]


#### Bypass Filters:
If direct redirection is blocked or sanitized, attackers may try bypass techniques, e.g.:

[https://www.yammer.com/redirect///?=///https://google.com///evil.com]



---

### Why This is Dangerous

- Enables phishing attacks by redirecting users to malicious websites.
- Can be used to bypass security controls.
- Damages brand trust if users are unknowingly redirected.

---

### How to Fix

To prevent Open Redirect vulnerabilities:
- Validate and whitelist redirect URLs.
- Avoid using user-controllable parameters for redirects without strict validation.
- Use relative paths rather than full URLs for internal redirects.
- Implement a fixed list of allowed redirect destinations.

---

### Tools for Detection

- **OpenRedireX** – Automated tool for detecting open redirect vulnerabilities  
  🔗 [GitHub Repository](https://github.com/devanshbatham/OpenRedireX)

---

### Additional Notes

Open Redirect is listed in the [OWASP Top 10](https://owasp.org/www-community/attacks/Unvalidated_Redirects_and_Forwards).  
It is both a security and usability risk that must be addressed during development and security reviews.


