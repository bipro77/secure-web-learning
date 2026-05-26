# A04:2025 — Cryptographic Failures

> Previously: A02:2021 — Cryptographic Failures

## Description

Failures related to cryptography (or its absence) that expose sensitive data in transit,
at rest, or during processing. This includes weak algorithms, poor key management,
missing encryption, and insecure transport configurations.

## OWASP Reference

https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/

## Key Concepts

- Weak or deprecated algorithms (MD5, SHA1, DES, RC4)
- Poor key generation, storage, or rotation
- Missing TLS / insecure transport
- Weak password hashing (unsalted MD5, SHA1)
- Hardcoded secrets or keys in source code
- Exposure of secrets in logs or error messages
- JWT algorithm confusion (none, RS256 → HS256)

## Lab Platforms

- [ ] TryHackMe
- [ ] PortSwigger

## Tools

- Burp Suite
- `openssl`
- JWT tooling (jwt.io, jwt_tool)
- Browser developer tools

## Workflow

1. Identify sensitive data flows (passwords, tokens, PII, payment data).
2. Inspect transport security (TLS version, cipher suites).
3. Review storage, token formats, and cookie attributes.
4. Validate whether cryptographic controls are appropriate and current.
5. Capture evidence in `screenshots/`.
6. Record findings in `notes.md`.
7. Document fixes in `remediation.md`.

## Checklist

- [ ] Sensitive data flows identified
- [ ] Transport security reviewed
- [ ] Token and cookie handling reviewed
- [ ] Password storage mechanism reviewed
- [ ] Screenshots captured
- [ ] Remediation written

## Notes

See `notes.md` for findings and methodology.

