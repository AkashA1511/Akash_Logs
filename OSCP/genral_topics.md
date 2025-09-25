````markdown
# OSCP Notes — General Topics

---

# WHOIS

**Purpose:** Get domain registration, registrar, creation/expiry dates, name servers, and contact info.

**Commands / Examples**
```bash
whois megacropone.com
````

**Tips:**

* Look for registrar, creation/expiry dates, and name servers.
* Historical WHOIS records can reveal previous owners or related domains.

---

# Google Hacking (Google Dorking)

**Purpose:** Use advanced Google operators to find sensitive files, directories, and data unintentionally exposed.

**Common dorks**

```text
site:megacropone.com filetype:txt
site:example.com inurl:"admin"
filetype:pdf "confidential"
intitle:"index of" "backup"
```

**Tips:**

* Combine `site:` with `filetype:` and `inurl:` to reduce noise.
* Be careful: scraping/searching at scale may trigger rate limits or violate ToS.

---

# Netcraft

**Purpose:** Passive reconnaissance — technology stack, hosting history, server OS, and historical screenshots.

**Usage:** Visit Netcraft and look up the domain to see historical data and hosting changes.

**Tips:**

* Useful to confirm if a site recently moved hosts (useful for finding stale DNS entries or leftover subdomains).

---

# GitHub / Open-source Code Dorking

**Purpose:** Find leaked API keys, credentials, or configuration files in public repositories.

**Examples**

```text
site:github.com "megacropone" "api_key"
site:github.com "password" "megacropone"
```

**Tips:**

* Search repo names, issues, Gists, and commit history.
* Check `.env`, config files, backup files, and CI/CD pipeline files (e.g., GitHub Actions secrets accidentally printed).

---

# Shodan

**Purpose:** Internet-wide device/service search — find exposed services, versions, banners, and default credentials.

**Common usage**

* Query by domain or IP.
* Filter by product (e.g., `nginx`, `Apache`), port (e.g., `:22`), or country.

**Tips:**

* Look for outdated versions, exposed management interfaces, and devices with default/weak credentials.

---

# SSH / TLS

**SSH (port 22)**

```bash
nmap -sV -p 22 --script=ssh2-enum-algos,target-ssh-hostkey <target>
ssh -v user@target
```

**Inspecting TLS**

* Check certificate details, issuer, validity dates, SANs.
* Look for expired certs, wildcard certs, or internal hostnames in SANs.

**Tools:** `openssl s_client`, `nmap --script ssl-*`, `sslyze`, `testssl.sh`.

---

# SecurityHeaders.com

**Purpose:** Quick public assessment of HTTP security headers (CSP, HSTS, X-Frame-Options, X-Content-Type-Options, etc.).

**Usage:** Visit `securityheaders.com` and enter the target domain.

**Tips:**

* Missing headers may point to insecure defaults or easy remediation paths.

---

# SSL Server Test (SSL Labs / sslservertest)

**Purpose:** In-depth TLS configuration assessment: protocol versions, cipher suites, certificate chain, and vulnerabilities (e.g., POODLE, BEAST).

**Usage:** Use SSL Labs (Qualys) or `sslservertest` to scan `https://megacropone.com`.

**Tips:**

* Look for weak ciphers, missing intermediate certs, and TLS version support.

---

# Quick Recon Checklist (One-liner workflow)

1. DNS + WHOIS:

   ```bash
   whois target.com
   dig @8.8.8.8 target.com ANY
   ```
2. Passive: Netcraft, securityheaders.com, SSL Labs.
3. Search engines: Google dorks, GitHub dorking.
4. Shodan: exposed services and versions.
5. Port scan:

   ```bash
   nmap -sC -sV -oA nmap/basic target
   ```
6. TLS/SSH checks:

   ```bash
   openssl s_client -connect target:443 -servername target
   nmap --script ssh-hostkey -p 22 target
   ```

---


