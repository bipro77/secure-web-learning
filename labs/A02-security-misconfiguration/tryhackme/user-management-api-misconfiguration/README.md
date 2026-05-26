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

## OWASP Reference

https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/
