# Contributing Guidelines

## Lab Filing Rules (OWASP 2025)

1. Always file labs under the **2025** category folder (A01–A10).
2. If a lab overlaps two categories, file under the **primary** category.
   Add a cross-reference note in the secondary category's `notes.md`.
3. **SSRF findings go under A01-broken-access-control** (not a separate folder).
4. Supply chain / dependency issues go under **A03-software-supply-chain-failures**.
5. Verbose error leakage / exception handling goes under **A10-mishandling-of-exceptional-conditions**.
6. Never upload: `.ovpn` files, flags, credentials, tokens, target IPs, cookies.

## Sensitive Data Rules

- Replace tokens, cookies, API keys, emails, usernames, hostnames, and IP addresses with placeholders.
- Use placeholders such as `<TARGET_HOST>`, `<RESOURCE_ID>`, `<SESSION_COOKIE>`, `<TOKEN>`, and `<REDACTED>`.
- Do not commit raw proxy history, packet captures, browser profiles, exported session data, VPN configs, or tool session files.
- Keep screenshots limited to intentionally vulnerable lab environments.
- Store only minimal, sanitized payload examples needed to explain the learning objective.

## Lab Documentation Standard

Every completed lab should include:

- Scope and authorization notes
- Platform and lab name
- Objective and vulnerability class
- Beginner-friendly explanation of the flaw
- Methodology and testing approach
- Sanitized evidence summaries (no real flags or credentials)
- Findings and impact assessment
- Remediation and validation steps
- Lessons learned and follow-up study references

## Naming Conventions

- Use lowercase kebab-case for all folder and file names.
- Prefix OWASP category folders with the 2025 OWASP ID (e.g., `A06-insecure-design`).
- Place platform-specific labs under `tryhackme/` or `portswigger/` within the category.
- Use descriptive topic names (e.g., `api-enumeration-verbose-error-leakage`).
- Keep reusable documentation templates under `methodology/templates/`.

## OWASP 2025 Quick Reference

| ID  | Category                                   |
|-----|--------------------------------------------|
| A01 | Broken Access Control *(+ SSRF)*           |
| A02 | Security Misconfiguration                  |
| A03 | Software Supply Chain Failures             |
| A04 | Cryptographic Failures                     |
| A05 | Injection                                  |
| A06 | Insecure Design                            |
| A07 | Authentication Failures                    |
| A08 | Software or Data Integrity Failures        |
| A09 | Security Logging and Alerting Failures     |
| A10 | Mishandling of Exceptional Conditions      |

## Authorized Environments Only

Use these notes, payloads, and workflows only against:

- TryHackMe rooms
- PortSwigger Web Security Academy labs
- OWASP Juice Shop (local)
- Burp Suite training labs
- Locally hosted intentionally vulnerable applications
- Systems you own or have **explicit written permission** to test
