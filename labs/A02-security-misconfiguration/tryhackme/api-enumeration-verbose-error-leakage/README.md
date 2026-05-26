# TryHackMe: API Enumeration and Verbose Error Leakage

## Overview

- Platform: TryHackMe
- OWASP category: A04 Insecure Design
- Related categories: A01 Broken Access Control, A05 Security Misconfiguration, A09 Logging and Monitoring Failures
- Status: Not started
- Documentation type: Sanitized learning writeup

## Focus Areas

- API enumeration
- Arbitrary ID access
- Insecure design
- Verbose error leakage
- Stack trace disclosure
- Improper exception handling
- Debug information exposure

## Objective

Practice identifying API design and error-handling weaknesses in an authorized TryHackMe lab. The goal is to understand how response differences, predictable identifiers, verbose exceptions, and debug output can reveal implementation details or unintended access patterns.

## Beginner-Friendly Explanation

API enumeration means learning how an API is structured by observing routes, methods, parameters, identifiers, status codes, and response patterns. Arbitrary ID access happens when changing an identifier such as `<RESOURCE_ID>` may expose data or behavior that should be restricted.

Verbose error leakage happens when an application reveals internal details such as stack traces, framework names, file paths, debug fields, or database errors. In a secure design, users receive enough information to understand that a request failed, while detailed diagnostics stay in protected server-side logs.

## Files

| File | Purpose |
| --- | --- |
| `methodology.md` | Test plan, scope, workflow, and evidence-handling rules |
| `findings.md` | Finding template for enumeration, ID access, verbose errors, and debug exposure |
| `mitigation.md` | Defensive controls and validation guidance |
| `lessons-learned.md` | Reflection, defensive perspective, and follow-up study |

## Safety Notes

- Use only the TryHackMe lab target assigned to the session.
- Replace hostnames, tokens, cookies, usernames, emails, resource IDs, and IP addresses with placeholders before committing notes.
- Do not commit raw proxy exports, screenshots containing secrets, real flags, target IPs, credentials, or real-world target data.
- Do not document active exploit chains.
