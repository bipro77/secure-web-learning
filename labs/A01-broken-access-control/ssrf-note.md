# SSRF — Now Part of A01 (2025)

In OWASP Top 10 2021, Server-Side Request Forgery (SSRF) was a standalone category (A10).

In OWASP Top 10 2025, SSRF has been consolidated into A01: Broken Access Control.
Rationale: SSRF is fundamentally an access control bypass at root cause.

Any SSRF labs or findings should be documented here under A01.

## Previous SSRF Lab Content

The former `A10-ssrf` folder has been renamed to
`A10-mishandling-of-exceptional-conditions` (a new 2025 category).

SSRF-specific notes, payloads, and labs should now live here under A01.

## Quick Reference

| Technique         | Description                                    |
|-------------------|------------------------------------------------|
| Basic SSRF        | Force server to fetch internal resources       |
| Blind SSRF        | No direct response; detect via out-of-band     |
| SSRF via headers  | `X-Forwarded-For`, `Host`, `Referer` abuse     |
| Cloud metadata    | AWS: `169.254.169.254`, GCP: `metadata.google` |

## OWASP Reference

- A01:2025 — https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/
- SSRF cheatsheet — https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html
