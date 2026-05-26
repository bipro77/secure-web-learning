# A05:2025 — Injection

> Previously: A03:2021 — Injection

## Description

Injection flaws occur when untrusted data is sent to an interpreter as part of a command
or query, allowing attackers to alter intended execution. SQL, OS, LDAP, NoSQL, and
template injection remain highly prevalent across web applications and APIs.

## OWASP Reference

https://owasp.org/Top10/2025/A05_2025-Injection/

## Key Concepts

- SQL injection (classic, blind, time-based, out-of-band)
- OS command injection
- NoSQL injection (MongoDB, Elasticsearch)
- LDAP injection
- Server-Side Template Injection (SSTI)
- Cross-Site Scripting (XSS — stored, reflected, DOM)
- XML/XPath injection
- Expression Language (EL) injection

## Lab Platforms

- [ ] TryHackMe
- [ ] PortSwigger

## Tools

- Burp Suite
- Browser developer tools
- `curl`
- `sqlmap` (authorized labs only)

## Workflow

1. Map all user-controlled input points.
2. Establish baseline application responses.
3. Send controlled payloads to identify behavior changes.
4. Confirm vulnerability with minimal, non-destructive evidence.
5. Capture evidence in `screenshots/`.
6. Record findings and remediation.

## Checklist

- [ ] Input points mapped
- [ ] Baseline responses captured
- [ ] Payloads tested safely
- [ ] Behavior confirmed
- [ ] Screenshots captured
- [ ] Remediation written

## Notes

See `notes.md` for findings and methodology.

