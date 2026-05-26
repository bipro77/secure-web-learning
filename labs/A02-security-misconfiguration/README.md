# A02:2025 — Security Misconfiguration

> Previously: A05:2021 — Security Misconfiguration

## Description

Improper security settings, default credentials, verbose error messages,
unnecessary exposed features, and unpatched environments create exploitable
attack surfaces that are entirely preventable.

## OWASP Reference

https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/

## Key Concepts

- Default credentials left unchanged
- Verbose errors exposing stack traces or internals
- Exposed admin interfaces
- Missing or misconfigured security headers
- Unnecessary services, features, or ports enabled
- Cloud/container misconfigurations

## Lab Platforms

- [ ] TryHackMe
- [ ] PortSwigger

## Workflow

1. Identify exposed surfaces and configuration clues.
2. Review headers, errors, methods, and default paths.
3. Test for unnecessary functionality.
4. Capture evidence in `screenshots/`.
5. Record findings and remediation.

## Checklist

- [ ] Headers reviewed
- [ ] Error handling reviewed
- [ ] Default paths checked
- [ ] Screenshots captured
- [ ] Remediation written

## Notes

See `notes.md` for findings and methodology.

