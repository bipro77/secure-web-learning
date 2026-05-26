# A02: Cryptographic Failures

## Objective

Identify failures related to protecting sensitive data in transit, at rest, or during processing.

## Scope

- Platform:
- Lab or room:
- Target URL:
- Sensitive data involved:

## Key Concepts

- Weak or missing encryption
- Poor key management
- Insecure transport security
- Weak password storage
- Exposure of secrets

## Tools

- Browser developer tools
- Burp Suite
- openssl
- jwt tooling

## Workflow

1. Identify sensitive data flows.
2. Inspect transport, storage, tokens, cookies, and responses.
3. Validate whether cryptographic controls are appropriate.
4. Capture evidence in `screenshots/`.
5. Record findings in `notes.md`.
6. Document fixes in `remediation.md`.

## Checklist

- [ ] Sensitive data identified
- [ ] Transport security reviewed
- [ ] Token and cookie handling reviewed
- [ ] Screenshots captured
- [ ] Remediation written

