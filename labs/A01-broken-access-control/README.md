# A01:2025 — Broken Access Control

> Previously: A01:2021 — Broken Access Control
> Also covers: SSRF (consolidated from A10:2021 — see `ssrf-note.md`)

## Description

Access control enforces policy such that users cannot act outside their intended permissions.
Failures typically lead to unauthorized information disclosure, modification, or destruction
of data, or performing business functions outside the user's intended limits.

In OWASP 2025, **SSRF is now consolidated here** as it is fundamentally an access control
bypass at its root cause.

## OWASP Reference

https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/

## Key Concepts

- Vertical privilege escalation
- Horizontal privilege escalation
- Insecure direct object references (IDOR)
- Missing server-side authorization checks
- Forced browsing
- SSRF (server-side request forgery)

## Lab Platforms

- [ ] TryHackMe
- [ ] PortSwigger

## Workflow

1. Map application roles and protected resources.
2. Identify authorization boundaries.
3. Attempt controlled access to unauthorized objects or functions.
4. Capture evidence in `screenshots/`.
5. Record findings in `notes.md`.
6. Document fixes in `remediation.md`.

## Checklist

- [ ] Scope confirmed
- [ ] Roles and permissions identified
- [ ] Test cases documented
- [ ] Screenshots captured
- [ ] Remediation written

## Notes

See `notes.md` for findings and methodology.
See `ssrf-note.md` for SSRF-specific guidance (merged from A10:2021).

