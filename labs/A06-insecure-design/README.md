# A06:2025 — Insecure Design

> Previously: A04:2021 — Insecure Design

## Description

Insecure design represents missing or ineffective security controls at the architectural
and design level — not implementation bugs. This includes failure to threat model,
missing abuse-case handling, unsafe business logic, and trust boundary mistakes.

## OWASP Reference

https://owasp.org/Top10/2025/A06_2025-Insecure_Design/

## Key Concepts

- Missing threat modeling
- Unsafe or bypassable business logic
- Weak abuse-case handling
- Missing rate limits or workflow guards
- Trust boundary mistakes
- API enumeration via predictable endpoints or response differences
- Verbose error leakage and debug information disclosure
- Improper exception handling exposing internals

## Lab Platforms

- [ ] TryHackMe
- [ ] PortSwigger

## Completed Labs

| Lab | Platform | Status |
|-----|----------|--------|
| [api-enumeration-verbose-error-leakage](tryhackme/api-enumeration-verbose-error-leakage/) | TryHackMe | ✅ |

## Tools

- Threat modeling notes / STRIDE
- Sequence diagrams
- Browser developer tools
- Burp Suite
- `curl`

## Workflow

1. Document the intended application workflow.
2. Identify assumptions and trust boundaries.
3. Test alternate paths, API methods, malformed inputs, and abuse cases.
4. Capture evidence in `screenshots/`.
5. Record findings in `notes.md`.
6. Document design-level mitigations in `remediation.md`.

## Checklist

- [ ] Workflow mapped
- [ ] Trust boundaries identified
- [ ] Abuse cases tested
- [ ] Verbose errors reviewed
- [ ] Screenshots captured
- [ ] Remediation written

## Notes

See `notes.md` for findings and methodology.

