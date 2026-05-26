# secure-web-learning

Portfolio-quality cybersecurity learning repository for OWASP Top 10 practice, web application testing notes, API security labs, secure coding references, and defensive remediation writeups.

This repository is designed to stay readable as it grows. Each lab is organized by OWASP category, platform, and topic so future TryHackMe, PortSwigger, Burp Suite, OWASP Juice Shop, and API security exercises can be added without changing the overall structure.

## Ethical Use Disclaimer

This repository is for defensive security education in authorized environments only. Suitable environments include TryHackMe rooms, PortSwigger Web Security Academy labs, OWASP Juice Shop, Burp Suite training labs, local intentionally vulnerable applications, owned systems, and systems where explicit written permission has been granted.

Do not use these notes, payloads, or workflows against third-party systems without authorization. Do not commit real flags, credentials, tokens, session cookies, private keys, target IP addresses, customer data, production screenshots, internal hostnames, raw exploit output, active exploit chains, or other sensitive material.

## Current Active Lab

Current lab:

```text
labs/A04-insecure-design/tryhackme/api-enumeration-verbose-error-leakage/
```

Coverage:

- API enumeration
- Arbitrary ID access
- Insecure design
- Verbose error leakage
- Stack trace disclosure
- Improper exception handling
- Debug information exposure

## Repository Structure

```text
secure-web-learning/
├── .gitignore
├── README.md
├── labs/
│   ├── A01-broken-access-control/
│   ├── A02-cryptographic-failures/
│   ├── A03-injection/
│   ├── A04-insecure-design/
│   ├── A05-security-misconfiguration/
│   ├── A06-vulnerable-components/
│   ├── A07-authentication-failures/
│   ├── A08-software-integrity-failures/
│   ├── A09-logging-monitoring-failures/
│   └── A10-ssrf/
├── methodology/
│   └── templates/
├── scripts/
└── tools/
```

Each OWASP category folder contains:

- `README.md` for category overview, objectives, and checklist
- `notes.md` for lab observations and learning notes
- `payloads.txt` for sanitized lab-only payload ideas
- `remediation.md` for mitigation and validation guidance
- `screenshots/.gitkeep` for approved lab screenshots
- `portswigger/.gitkeep` for PortSwigger lab notes
- `tryhackme/.gitkeep` for TryHackMe lab notes

## OWASP Top 10 Index

| ID | Category | Folder |
| --- | --- | --- |
| A01 | Broken Access Control | `labs/A01-broken-access-control/` |
| A02 | Cryptographic Failures | `labs/A02-cryptographic-failures/` |
| A03 | Injection | `labs/A03-injection/` |
| A04 | Insecure Design | `labs/A04-insecure-design/` |
| A05 | Security Misconfiguration | `labs/A05-security-misconfiguration/` |
| A06 | Vulnerable Components | `labs/A06-vulnerable-components/` |
| A07 | Authentication Failures | `labs/A07-authentication-failures/` |
| A08 | Software Integrity Failures | `labs/A08-software-integrity-failures/` |
| A09 | Logging and Monitoring Failures | `labs/A09-logging-monitoring-failures/` |
| A10 | Server-Side Request Forgery | `labs/A10-ssrf/` |

## Documentation Standard

Every completed lab should include:

- Scope and authorization notes
- Platform and lab name
- Objective and vulnerability class
- Beginner-friendly explanation
- Methodology and testing approach
- Sanitized evidence summaries
- Findings and impact
- Remediation and validation steps
- Lessons learned and follow-up study

## Naming Conventions

- Use lowercase kebab-case for folders.
- Prefix OWASP category folders with the OWASP ID.
- Place platform-specific labs under `tryhackme/` or `portswigger/`.
- Use clear topic names such as `api-enumeration-verbose-error-leakage`.
- Keep reusable documentation under `methodology/templates/`.

## Sensitive Data Rules

- Replace tokens, cookies, API keys, emails, usernames, hostnames, and IP addresses with placeholders.
- Use placeholders such as `<TARGET_HOST>`, `<RESOURCE_ID>`, `<SESSION_COOKIE>`, `<TOKEN>`, and `<REDACTED>`.
- Do not commit raw proxy history, packet captures, browser profiles, exported session data, VPN configs, or tool session files.
- Keep screenshots limited to intentionally vulnerable lab environments.
- Store only minimal, sanitized payload examples needed to explain the learning objective.

## Learning Tracks

- OWASP Top 10
- TryHackMe
- PortSwigger Web Security Academy
- Burp Suite labs
- OWASP Juice Shop
- API security labs
- Secure coding notes
- Web application testing methodology
