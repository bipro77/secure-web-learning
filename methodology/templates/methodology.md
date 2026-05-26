# Methodology Template

## Lab Context

- Platform:
- Lab name:
- OWASP category:
- Date:
- Status:
- Authorization scope:

## Objective

Describe the behavior being tested and the learning goal.

## Beginner-Friendly Explanation

Explain the vulnerability class in plain language. Focus on what the issue means, why it matters, and what a secure design should do instead.

## Tools

- Browser developer tools:
- Burp Suite:
- curl:
- Other:

## Rules of Engagement

- Testing is limited to the authorized lab environment.
- Do not store real flags, credentials, target IP addresses, tokens, or session data.
- Sanitize all requests, responses, screenshots, and tool output before committing.
- Avoid documenting active exploit chains.

## Workflow

1. Confirm authorization and lab boundaries.
2. Map visible functionality, routes, roles, and trust boundaries.
3. Capture baseline requests and responses.
4. Change one variable at a time.
5. Compare status codes, headers, response bodies, and application behavior.
6. Record sanitized evidence.
7. Document impact, remediation, and validation.

## Evidence Handling

- Replace secrets and identifiers with placeholders.
- Store screenshots only when they are safe for public sharing.
- Do not commit raw proxy exports, packet captures, VPN files, logs, or session files.

## Remediation Mapping

- Root cause:
- Recommended controls:
- Validation approach:
- Follow-up study:
