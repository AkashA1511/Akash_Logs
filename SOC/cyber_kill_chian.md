# Cyber Kill Chain

1. **Reconnaissance**

   * Scanning, finding information about devices, systems, and employees.
   * Gathering data for planning the attack.

2. **Weaponization**

   * Creating exploits or scripts after collecting information.
   * Malicious files (e.g., PDF, Excel) prepared for delivery.

3. **Delivery**

   * Methods to deliver the payload:

     * USB drops
     * Email (phishing campaigns)
     * Social media ads (job openings, discounts, giveaways)
     * Direct exploitation of internet-facing servers (web apps, RDP, etc.)

4. **Exploitation**

   * Executing the attack by exploiting vulnerabilities.
   * Common causes: unpatched OS, outdated versions, misconfigurations.

5. **Installation**

   * Installing malware/backdoors after successful exploitation.
   * Malware isn’t always part of the payload; exploitation can be used to download malicious software for persistence.

6. **Command & Control (C2)**

   * Attacker establishes remote access to the compromised system.
   * Persistence is maintained so even if malware is removed, access can be regained.

7. **Actions on Objectives**

   * Final goal of the attacker:

     * Data theft
     * Ransomware
     * Destruction or sabotage

---

# MITRE ATT\&CK Framework

The MITRE ATT\&CK framework is a knowledge base of adversary tactics, techniques, and procedures (TTPs). It helps organizations analyze cyber threats, detect attacks, and plan defenses.

* **MITRE ATT\&CK** is maintained by MITRE, a non-profit sponsored by the U.S. government.
* It is widely used for **threat detection, incident response, and red/purple team exercises**.

## Components of MITRE ATT\&CK

1. **Tactics**

   * The *why* behind an attacker’s action (e.g., Initial Access, Persistence, Execution).
   * There are 14 tactics in the Enterprise matrix.

2. **Techniques**

   * The *how* — specific methods used to achieve a tactic.
   * Includes more than 200 techniques and sub-techniques (e.g., Spearphishing, Valid Accounts, Scheduled Task/Job).

3. **Matrix**

   * A visual grid mapping tactics (columns) against techniques (rows).
   * Shows how adversaries progress through different phases of an attack.

---

