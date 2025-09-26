````markdown
# DNS Enumeration — Practical Notes & Commands

Concise, focused DNS enumeration cheat-sheet. Paste this directly into a markdown file.

---

## Basic `host` commands

Use `host` for quick lookups.

```bash
# A record / basic lookup
host target.com

# Mail services (MX records)
host -t mx target.com

# TXT records (SPF, DKIM, other text records)
host -t txt target.com

# NS records (nameservers)
host -t ns target.com
````

---

## `dig` quick checks (recommended additions)

`dig` is more flexible for deeper DNS checks.

```bash
# Short answer for A records
dig +short target.com

# Full query for a specific record type
dig target.com MX

# Try zone transfer from a nameserver (AXFR)
dig @ns1.example.com target.com AXFR

# ANY query (note: many servers block ANY)
dig target.com ANY
```

**Tip:** If `AXFR` succeeds it usually means zone transfer is misconfigured — collect everything returned.

---

## Check for wildcard DNS

Wildcard entries (e.g., `*.target.com`) can make bruteforce noisy; detect them first:

```bash
# query a random subdomain that shouldn't exist
host random-subdomain-check-$(date +%s).target.com
```

If this returns a valid A record repeatedly, the domain likely uses wildcard DNS.

---

## Bruteforcing subdomains — small robust bash script

Create a wordlist file, e.g. `listdns.txt`:

```text
www
ftp
mail
owa
proxy
router
dev
staging
admin
api
```

Simple, reliable script that handles wildcard detection and deduplicates results:

```bash
#!/bin/bash
TARGET="target.com"
WL="listdns.txt"
RESOLVER="1.1.1.1"   # optional: use a specific resolver
OUT="dns_results.txt"
TMP="._wildcard_check.tmp"

# Clean previous results
> "$OUT"

# Detect wildcard IPs (query several impossible names)
for i in 1 2 3; do
  host "nonexistent-$i.$TARGET" $RESOLVER >> "$TMP" 2>/dev/null
done
WILD_IPS=$(awk '/has address/ {print $4} ' "$TMP" | sort -u)
rm -f "$TMP"

# Bruteforce
while read -r sub; do
  fqdn="${sub}.${TARGET}"
  ip=$(dig +short @"$RESOLVER" "$fqdn" A | sed '/^$/d' | tr '\n' ' ' | sed 's/ $//')
  if [ -n "$ip" ]; then
    # skip results that match wildcard IPs (if any)
    skip=false
    for w in $WILD_IPS; do
      if echo "$ip" | grep -q "$w"; then skip=true; fi
    done
    if [ "$skip" = false ]; then
      echo -e "$fqdn\t$ip" | tee -a "$OUT"
    fi
  fi
done < "$WL"

# Deduplicate
sort -u "$OUT" -o "$OUT"
echo "Results saved to $OUT"
```

Make executable: `chmod +x dns_bruteforce.sh` and run `./dns_bruteforce.sh`.

---

## Faster / parallel approach (xargs)

If you have a large list, use `xargs` to parallelize queries:

```bash
cat listdns.txt | xargs -I{} -P30 bash -c 'dig +short {}.target.com | sed "/^$/d" | awk "{print \"{}.target.com\t\" \$0}"' >> parallel_results.txt
```

`-P30` runs up to 30 parallel processes — tune to avoid hitting rate limits.

---

## Zone transfer attempts (AXFR) and nameserver enumeration

1. Enumerate NS records:

```bash
dig +short NS target.com
```

2. Attempt AXFR against each NS:

```bash
for ns in $(dig +short NS target.com); do
  echo "Trying AXFR on $ns"
  dig @"$ns" target.com AXFR
done
```

If any server allows AXFR, you'll often get the entire zone.

---

## Reverse DNS / PTR sweep (given IP range)

If you have an IP range, try reverse lookups to find hostnames:

```bash
# Example single IP
host 10.1.2.3

# For a list of IPs
for ip in $(cat ips.txt); do host $ip; done
```

---

## Useful tools / repos (as you mentioned)

* `darkoperator/dnsrecon` — automated DNS enumeration and bruteforcing.
  GitHub: `github.com/darkoperator/dnsrecon`

* `dbsenum2` — DNS enumeration scripts and helpers.
  GitHub: `github.com/dbsenum2`  *(use the exact repo path when cloning)*

Other well-known tools you may consider: `subfinder`, `amass`, `massdns`, `dnsenum`, `fierce`. (Use only authorized targets.)

---

## Operational tips (no fluff)

* Always confirm authorization before scanning.
* Start passive (OSINT) before active bruteforcing.
* Respect rate limits and avoid noisy scans on production targets.
* Use a reliable resolver (Cloudflare `1.1.1.1`, Google `8.8.8.8`) to avoid local caching issues.
* Store outputs in timestamped files and deduplicate (`sort -u`) before reporting.
* If you find a wildcard, switch strategy: rely more on passive discovery, certificate transparency logs, GitHub dorking, or use subdomain enumeration tools that detect wildcards.

---

## Minimal one-page checklist

1. `host target.com` / `dig +short target.com`
2. `host -t ns target.com` → enumerate NS
3. Try `AXFR` on each NS (`dig @ns target.com AXFR`)
4. Detect wildcard DNS (`host random-name.target.com`)
5. Bruteforce with `listdns.txt` (script above)
6. Parallelize if needed (`xargs -P`)
7. Check GitHub / CT logs / passive sources if bruteforce noisy
8. Save and deduplicate results

---
