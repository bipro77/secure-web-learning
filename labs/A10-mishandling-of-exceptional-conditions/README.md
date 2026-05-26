# A10:2025 — Mishandling of Exceptional Conditions

> New category in 2025 (replaces A10:2021 — SSRF, which moved to A01)

## Description

Applications that fail to gracefully handle unexpected inputs, errors, or edge cases
can expose internal implementation details, crash in exploitable ways, or enter
undefined states. Improper exception handling is a root cause of information disclosure,
denial of service, and logic bypass vulnerabilities.

> **Note on SSRF:** SSRF (formerly A10:2021) has been consolidated into
> A01:2025 — Broken Access Control. See `../A01-broken-access-control/ssrf-note.md`.

## OWASP Reference

https://owasp.org/Top10/2025/A10_2025-Mishandling_of_Exceptional_Conditions/

## Key Concepts

- Unhandled exceptions leaking stack traces or framework internals
- Verbose error messages with file paths, SQL queries, or config details
- Improper null/edge-case handling leading to crashes or bypasses
- Error-based information disclosure (timing, content, status differences)
- Business logic bypass via exception paths
- Debug mode left enabled in production

## Lab Platforms

- [ ] TryHackMe
- [ ] PortSwigger

## Related Labs (Filed Here)

| Lab | Platform | Status |
|-----|----------|--------|
| *Verbose error leakage labs* | — | See A06 for api-enumeration lab |

## Tools

- Burp Suite (Repeater — send malformed inputs)
- Browser developer tools
- `curl`

## Workflow

1. Identify input points that may trigger unexpected behavior.
2. Send malformed, boundary, and null inputs.
3. Review error responses for internal information disclosure.
4. Test alternate HTTP methods, encoding, and content types.
5. Capture evidence in `screenshots/`.
6. Record findings and remediation.

## Checklist

- [ ] Input points for edge cases identified
- [ ] Malformed inputs tested
- [ ] Error responses reviewed for leakage
- [ ] Debug mode / verbose errors checked
- [ ] Screenshots captured
- [ ] Remediation written

## Notes

See `notes.md` for findings and methodology.

