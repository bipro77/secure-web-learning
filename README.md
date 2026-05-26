# secure-web-learning

Personal cybersecurity learning repository.
Structured around **OWASP Top 10 2025** (migrated from 2021).

Portfolio-quality writeups for web application testing, API security labs, secure coding references, and defensive remediation notes. Each lab is organized by OWASP 2025 category, platform, and topic for long-term maintainability.

## Ethical Use Disclaimer

This repository is for defensive security education in authorized environments only. Suitable environments include TryHackMe rooms, PortSwigger Web Security Academy labs, OWASP Juice Shop, Burp Suite training labs, local intentionally vulnerable applications, owned systems, and systems where explicit written permission has been granted.

Do not use these notes, payloads, or workflows against third-party systems without authorization. Do not commit real flags, credentials, tokens, session cookies, private keys, target IP addresses, customer data, production screenshots, internal hostnames, raw exploit output, active exploit chains, or other sensitive material.

## OWASP Top 10 2025

| ID  | Category                                   | Folder                                          |
|-----|--------------------------------------------|-------------------------------------------------|
| A01 | Broken Access Control *(+ SSRF)*           | `labs/A01-broken-access-control/`               |
| A02 | Security Misconfiguration                  | `labs/A02-security-misconfiguration/`           |
| A03 | Software Supply Chain Failures             | `labs/A03-software-supply-chain-failures/`      |
| A04 | Cryptographic Failures                     | `labs/A04-cryptographic-failures/`              |
| A05 | Injection                                  | `labs/A05-injection/`                           |
| A06 | Insecure Design                            | `labs/A06-insecure-design/`                     |
| A07 | Authentication Failures                    | `labs/A07-authentication-failures/`             |
| A08 | Software or Data Integrity Failures        | `labs/A08-software-or-data-integrity-failures/` |
| A09 | Security Logging and Alerting Failures     | `labs/A09-security-logging-and-alerting-failures/` |
| A10 | Mishandling of Exceptional Conditions      | `labs/A10-mishandling-of-exceptional-conditions/` |

## Version History

| Version | Date       | Notes                                      |
|---------|------------|--------------------------------------------|
| 2021    | 2024       | Initial structure (OWASP Top 10 2021)      |
| 2025    | 2025-05-25 | Migrated to OWASP Top 10 2025              |

## Current Active Lab

```text
labs/A06-insecure-design/tryhackme/api-enumeration-verbose-error-leakage/
```

Coverage: API enumeration · Arbitrary ID access · Verbose error leakage ·
Stack trace disclosure · Improper exception handling · Debug information exposure

## Repository Structure

```text
secure-web-learning/
├── .gitignore
├── README.md
├── labs/
│   ├── A01-broken-access-control/      ← also covers SSRF (see ssrf-note.md)
│   ├── A02-security-misconfiguration/
│   ├── A03-software-supply-chain-failures/
│   ├── A04-cryptographic-failures/
│   ├── A05-injection/
│   ├── A06-insecure-design/
│   ├── A07-authentication-failures/
│   ├── A08-software-or-data-integrity-failures/
│   ├── A09-security-logging-and-alerting-failures/
│   └── A10-mishandling-of-exceptional-conditions/
├── methodology/
│   └── templates/
├── scripts/
└── tools/
```

Each OWASP category folder contains:

- `README.md` — category overview, 2025 mapping, objectives, and checklist
- `notes.md` — lab observations and learning notes
- `payloads.txt` — sanitized lab-only payload ideas
- `remediation.md` — mitigation and validation guidance
- `screenshots/.gitkeep` — approved lab screenshots
- `portswigger/` — PortSwigger Web Security Academy lab notes
- `tryhackme/` — TryHackMe lab notes

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
