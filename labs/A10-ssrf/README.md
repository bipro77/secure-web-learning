# A10: Server-Side Request Forgery (SSRF)

## Objective

Understand how server-side request functionality can be abused to access unintended internal or external resources.

## Scope

- Platform:
- Lab or room:
- Target URL:
- Request feature:

## Key Concepts

- URL fetchers
- Internal network access
- Cloud metadata services
- URL parser bypasses
- Allowlist and denylist weaknesses

## Tools

- Burp Suite
- Collaborator-style endpoint for authorized labs
- curl
- Browser developer tools

## Workflow

1. Identify server-side URL fetching behavior.
2. Establish controlled callbacks or response differences.
3. Test internal, loopback, and metadata-style targets only in authorized labs.
4. Capture evidence in `screenshots/`.
5. Record findings and remediation.

## Checklist

- [ ] URL-fetching feature identified
- [ ] Callback or response behavior confirmed
- [ ] Bypass attempts documented
- [ ] Screenshots captured
- [ ] Remediation written

