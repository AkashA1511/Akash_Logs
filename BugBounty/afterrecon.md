### **1. Asset Triage**

From your recon list (`alive_subdomains.txt`, `urls.txt`):

- ✅ Prioritize:
    - Login/signup pages
    - Admin panels
    - `.dev`, `.staging`, `.test` subdomains
    - Pages with params (`?id=`, `?q=`, `?redirect=`)
- ✅ Use:
    - `httpx -title -sc -content-length -tech-detect`
    - Manually open the juicy ones in browser/Burp

---

###  **2. Parameter Discovery**

Find as many injectable points as possible:

| Tool | Use |
| --- | --- |
| `katana` | Crawls all links/params from a URL |
| `waybackurls` | Gets historical params from Internet Archive |
| `gau` | Similar to wayback but faster |
| `arjun` | Bruteforce parameters |
| `ParamSpider` | Scrape JS + find hidden params |

🔹 Pipe to `gf` (like `gf xss`) to filter vuln-relevant URLs
🔹 Save and manage: `params.txt`

---

###  **3. Manual Testing in Burp**

Load target in Burp Suite → Intercept request → Send to Repeater.

Test for:

| Vuln | Try this |  |
| --- | --- | --- |
| **XSS** | Inject: `<script>alert(1)</script>`, `"><img src=x onerror=alert(1)>` |  |
| **SQLi** | Try `' OR 1=1--`, `UNION SELECT NULL,NULL--` |  |
| **Open Redirect** | `?next=https://evil.com`, `?url=//evil.com` |  |
| **IDOR** | Change `user_id=123` to `122`, or access other users’ data |  |
| **SSRF** | Use Burp Collaborator in `?url=` params |  |
| **Command Injection** | Use `;ls`, \` | whoami\` in input fields or params |

---

###  **4. JS File Hunting**

JS often leaks gold.

| Tool | Use |  |  |  |
| --- | --- | --- | --- | --- |
| `katana` / `gau` | Extract `.js` links |  |  |  |
| `LinkFinder` | Extract endpoints and URLs from JS |  |  |  |
| `SecretFinder` | Extract secrets like API keys, JWTs |  |  |  |
| `grep` manually | \`grep -Ei "api\_key | secret | token | auth" \*.js\` |

 Look for:

- `/api/` routes
- Unused endpoints
- Tokens, hardcoded credentials

---

###  **5. Directory & File Bruteforce**

Use tools like:

```bash
ffuf -u <https://target.com/FUZZ> -w ~/wordlists/raft-small-directories.txt -mc 200

```

Discover:

- `/admin`
- `/dashboard`
- `/uploads`
- Hidden backup files: `index.php~`, `.git`, `.env`