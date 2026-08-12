# DVWA Finding Chain: Stored XSS to Persistent RCE

### A pentest-styled deliverable documenting a 4-finding vulnerability chain on DVWA — from a single unsanitised comment field to a persistent server-level backdoor

**Author:** Kofi Asibey-Kitiabi
**GitHub:** [Mastertactician23](https://github.com/Mastertactician23/)
**Date:** August 2026
**Target:** DVWA (`vulnerables/web-dvwa:latest`, security level: low)

---

## The Chain

```
Finding 1: Stored XSS → session token theft (A03:2021 Critical)
    │  Malicious guestbook comment steals admin cookie from every visitor
    ▼
Finding 2: SQL Injection → full credential extraction (A03:2021 Critical)
    │  UNION SELECT pulls all usernames + MD5 hashes from the users table
    ▼
Finding 3: OS Command Injection → Remote Code Execution (A03:2021 Critical)
    │  Ping utility executes arbitrary OS commands as www-data
    ▼
Finding 4: Unrestricted File Upload → Persistent Web Shell (A05:2021 Critical)
       PHP shell uploaded, executes independently of any session forever
```

**Business impact:** An attacker submitting one malicious guestbook comment achieves total server compromise — credential theft, remote code execution, and a persistent backdoor — starting with no valid account and no prior access.

---

## Deliverables

| File | Purpose |
|------|---------|
| `DVWA_PENTEST_REPORT.md` | Full pentest-styled report: exec summary, methodology, 4 findings with CVSS, repro steps, evidence, root cause, remediation |
| `dvwa_poc_exploit.py` | Python PoC automating the full 4-finding chain end to end |
| `screenshots/` | Per-finding visual evidence |

---

## How to Reproduce

**Start DVWA:**
```bash
docker run -d --name dvwa --network soc-lab -p 8090:80 vulnerables/web-dvwa
```

**Wait ~15 seconds, then set up:**
- Visit `http://localhost:8090/setup.php` → click **Create / Reset Database**
- Log in: `admin` / `password`
- Go to DVWA Security → set to **Low** → Submit

**Run the PoC chain:**
```bash
pip install requests colorama --break-system-packages
python3 dvwa_poc_exploit.py --target http://dvwa:8090
```

---

## MITRE ATT&CK Mapping

| Finding | Technique | ID |
|---------|-----------|-----|
| Stored XSS | Drive-by Compromise | T1189 |
| SQL Injection | Exploit Public-Facing Application | T1190 |
| Command Injection | Command and Scripting Interpreter | T1059 |
| File Upload Web Shell | Server Software Component: Web Shell | T1505.003 |

---

## Why DVWA — Not Juice Shop Again

The [Juice Shop chain](https://github.com/Mastertactician23/juiceshop-owasp-finding-chain) ended at data exfiltration. This chain ends at **server-level code execution and persistence** — a materially different and higher-severity impact category. Two projects, two different vulnerability classes, two different target applications, clearly distinct narrative arcs.

---

## Skills Demonstrated

- Manual web application penetration testing
- Stored XSS exploitation and session theft methodology
- UNION-based SQL injection for data extraction
- OS command injection chaining and RCE confirmation
- Web shell deployment and persistence verification
- CVSS 3.1 scoring and severity classification
- Professional pentest report writing
- Python PoC development for reproducible evidence

---

*github.com/Mastertactician23/dvwa-rce-finding-chain*
