# Hyperlink Injection

Hyperlink Injection is a security vulnerability that allows attackers to inject malicious URLs into web applications or emails.  
This can trick victims into clicking harmful links, leading to phishing attacks, malware installation, or unauthorized access.

---

## Example: HYPERLINK INJECTION / EMAIL INJECTION

An attacker injects a malicious link into an input field that later gets sent via email.

Example email received by the victim:

> Hello, [https://attacker.com](https://attacker.com) — visit this link to grab $1000!

---

### Attack Scenario

Pages susceptible to hyperlink injection:
- Contact Us page  
- Signup page  
- Login page  
- Inviting page  
- Demo page  

Steps:
1. There is a form with **First Name** and **Last Name** fields.
2. Attacker enters:
   - First Name: `https://www.evil.com`
   - Last Name: `Click on this to link grab a car or a bounty or some lottery`
3. Enter other optional inputs.
4. Click **Send**.
5. Victim receives email containing malicious hyperlink — hyperlink injection is executed.

---

### How the Vulnerability Works

- Form inputs are not sanitized.
- Input values containing URLs are directly inserted into email content.
- Victims clicking the injected link unknowingly get redirected to malicious websites.

---

### How to Fix

Prevent hyperlink injection by:
- Validating and sanitizing all user inputs.
- Blocking dangerous characters and patterns such as:  
  `, /, ://, .com` and other URL-specific symbols.
- Implementing strict input restrictions and escaping HTML entities.
- Using a whitelist of allowed characters for input fields.
- Avoiding direct insertion of user input into emails without sanitization.

---

### Additional Notes

- Hyperlink injection is a subset of **input injection vulnerabilities** and closely related to **HTML injection** and **email injection**.
- It can be exploited for phishing, spreading malware, and stealing sensitive information.
- Always use secure coding practices to mitigate such risks.

---
