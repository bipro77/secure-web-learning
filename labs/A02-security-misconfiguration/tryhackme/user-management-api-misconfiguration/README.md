# Lab: User Management API Misconfiguration

## Classification

| Field        | Value                                      |
|--------------|--------------------------------------------|
| OWASP 2025   | A02 — Security Misconfiguration            |
| Platform     | TryHackMe                                  |
| Port         | 5002                                       |
| Difficulty   | Beginner                                   |
| Status       | In Progress                                |

## Objective

Developers left too many traces in their User Management APIs.
Identify what is exposed, why it is a misconfiguration, and what an attacker could do with it.

## Vulnerability Class

- Verbose error messages exposing stack traces or system details
- Exposed API endpoints without authentication
- Debug or development traces left in production
- Unnecessary information disclosure

## Files

| File               | Purpose                              |
|--------------------|--------------------------------------|
| methodology.md     | Step-by-step testing approach        |
| findings.md        | Discovered vulnerabilities           |
| mitigation.md      | How to fix each finding              |
| lessons-learned.md | Key takeaways                        |
| screenshots/       | Evidence captured during testing     |

## Screenshots

| File                                          | Phase   | Finding                              |
|-----------------------------------------------|---------|--------------------------------------|
| `01-recon-nmap-service-scan.png`              | Recon   | nmap reveals Werkzeug/3.1.3 on 5002–5004 |
| `02-enum-api-user-123-idor.png`               | Enum    | /api/user/123 returns data unauthenticated |
| `03-enum-api-user-999999-idor-no-auth.png`    | Enum    | /api/user/999999 processed with no auth check |
| `04-exploit-api-negative-id-verbose-error.png`| Exploit | /api/user/-1 triggers verbose 500 error |
| `05-exploit-api-xyz-stack-trace-disclosure.png`| Exploit | /api/user/xyz triggers full stack trace + flag |

## OWASP Reference

https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/
