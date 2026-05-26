# Labs

Labs are organized by OWASP Top 10 category. Each category can contain platform-specific writeups for TryHackMe, PortSwigger Web Security Academy, OWASP Juice Shop, Burp Suite labs, and local practice environments.

## Structure

```text
labs/
├── A01-broken-access-control/
├── A02-cryptographic-failures/
├── A03-injection/
├── A04-insecure-design/
├── A05-security-misconfiguration/
├── A06-vulnerable-components/
├── A07-authentication-failures/
├── A08-software-integrity-failures/
├── A09-logging-monitoring-failures/
└── A10-ssrf/
```

## Category Folder Standard

Each OWASP category includes:

- `README.md`
- `notes.md`
- `payloads.txt`
- `remediation.md`
- `screenshots/.gitkeep`
- `portswigger/.gitkeep`
- `tryhackme/.gitkeep`

## Adding a New Lab

1. Choose the closest OWASP category.
2. Create a topic folder under the platform directory.
3. Copy the reusable templates from `../methodology/templates/`.
4. Sanitize all evidence before committing.
5. Keep notes educational and defensive.

Example:

```text
labs/A04-insecure-design/tryhackme/api-enumeration-verbose-error-leakage/
├── README.md
├── findings.md
├── lessons-learned.md
├── methodology.md
└── mitigation.md
```
