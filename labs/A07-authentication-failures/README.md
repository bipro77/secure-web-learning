# A07:2025 — Authentication Failures

> Previously: A07:2021 — Identification and Authentication Failures

## Description

Weaknesses in authentication, session management, and identity verification that allow
attackers to assume other users' identities temporarily or permanently, compromise
passwords, keys, and session tokens.

## OWASP Reference

https://owasp.org/Top10/2025/A07_2025-Authentication_Failures/

## Key Concepts

- Weak or default credential policies
- Broken session management (predictable tokens, no expiry)
- Credential stuffing and brute-force exposure
- Insecure password reset flows
- Missing or bypassable multi-factor authentication
- Session fixation
- Exposed session tokens in URLs or logs

## Lab Platforms

- [ ] TryHackMe
- [ ] PortSwigger

## Tools

- Browser developer tools
- Burp Suite (Intruder for authorized rate-limit tests)
- `curl`

## Workflow

1. Map all authentication and credential recovery flows.
2. Review session cookie attributes (HttpOnly, Secure, SameSite, expiry).
3. Test rate limiting and lockout behavior in authorized scope.
4. Review MFA implementation for bypass possibilities.
5. Capture evidence in `screenshots/`.
6. Record findings and remediation.

## Checklist

- [ ] Auth flows mapped
- [ ] Session behavior reviewed
- [ ] Password reset flow reviewed
- [ ] MFA reviewed
- [ ] Screenshots captured
- [ ] Remediation written

## Notes

See `notes.md` for findings and methodology.

