# Labs

Labs are organized by **OWASP Top 10 2025** category. Each category contains
platform-specific writeups for TryHackMe, PortSwigger Web Security Academy,
OWASP Juice Shop, Burp Suite labs, and local practice environments.

## Structure (OWASP 2025)

```text
labs/
├── A01-broken-access-control/            ← also covers SSRF (see ssrf-note.md)
├── A02-security-misconfiguration/
├── A03-software-supply-chain-failures/
├── A04-cryptographic-failures/
├── A05-injection/
├── A06-insecure-design/
├── A07-authentication-failures/
├── A08-software-or-data-integrity-failures/
├── A09-security-logging-and-alerting-failures/
└── A10-mishandling-of-exceptional-conditions/
```

## OWASP 2021 → 2025 Mapping Reference

| 2021 Folder                       | 2025 Folder                                    |
|-----------------------------------|------------------------------------------------|
| A01-broken-access-control         | A01-broken-access-control *(kept + SSRF added)*|
| A02-cryptographic-failures        | A04-cryptographic-failures                     |
| A03-injection                     | A05-injection                                  |
| A04-insecure-design               | A06-insecure-design                            |
| A05-security-misconfiguration     | A02-security-misconfiguration                  |
| A06-vulnerable-components         | A03-software-supply-chain-failures             |
| A07-authentication-failures       | A07-authentication-failures *(kept)*           |
| A08-software-integrity-failures   | A08-software-or-data-integrity-failures        |
| A09-logging-monitoring-failures   | A09-security-logging-and-alerting-failures     |
| A10-ssrf                          | → merged into A01; A10 = new category          |

## Category Folder Standard

Each OWASP category includes:

- `README.md` — category overview with 2025 description and OWASP reference link
- `notes.md` — lab observations and learning notes
- `payloads.txt` — sanitized lab-only payload ideas
- `remediation.md` — mitigation and validation guidance
- `screenshots/.gitkeep` — approved lab screenshots
- `portswigger/` — PortSwigger Web Security Academy lab notes
- `tryhackme/` — TryHackMe lab notes

## Lab Filing Rules (OWASP 2025)

1. Always file labs under the **2025** category folder (A01–A10).
2. If a lab overlaps two categories, file under the **primary** category.
   Add a cross-reference note in the secondary category's `notes.md`.
3. **SSRF findings go under A01-broken-access-control** (not a separate folder).
4. Supply chain / dependency issues go under **A03-software-supply-chain-failures**.
5. Verbose error leakage / exception handling goes under **A10-mishandling-of-exceptional-conditions**.
6. Never commit: `.ovpn` files, flags, credentials, tokens, target IPs, session cookies.

## Adding a New Lab

1. Choose the closest OWASP 2025 category.
2. Create a topic folder under the platform directory (`tryhackme/` or `portswigger/`).
3. Copy the reusable templates from `../methodology/templates/`.
4. Sanitize all evidence before committing.
5. Keep notes educational and defensive.

Example:

```text
labs/A06-insecure-design/tryhackme/api-enumeration-verbose-error-leakage/
├── README.md
├── findings.md
├── lessons-learned.md
├── methodology.md
└── mitigation.md
```
