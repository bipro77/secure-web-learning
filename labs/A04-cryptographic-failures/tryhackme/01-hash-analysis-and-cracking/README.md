# Lab: Client-Side Key Exposure and Document Decryption

## Classification

| Field | Value |
| --- | --- |
| OWASP 2025 | A04 - Cryptographic Failures |
| Platform | TryHackMe |
| Primary service | Browser-based web application |
| Difficulty | Beginner |
| Status | Completed |

## Objective

Identify how client-side cryptography fails when encryption keys and decryption logic are
delivered to the browser with the protected data.

## Vulnerability Class

- Hardcoded cryptographic key exposed in client-side source
- Decryption logic executed in the browser
- Sensitive encrypted document accessible to unauthenticated or low-privilege users
- No effective server-side authorization around protected data
- Missing key management, key rotation, and secret isolation

## Files

| File | Purpose |
| --- | --- |
| `methodology.md` | Step-by-step testing approach |
| `findings.md` | Discovered vulnerabilities |
| `mitigation.md` | How to fix each finding |
| `lessons-learned.md` | Key takeaways |
| `screenshots/` | Evidence captured during testing |

## Screenshots

| File | Phase | Finding |
| --- | --- | --- |
| `screenshots/01-homepage.png` | Recon | Target application entry point |
| `screenshots/02-page-source.png` | Source review | Client-side code and referenced assets reviewed |
| `screenshots/03-developer-tools.png` | Analysis | Browser developer tools used to inspect scripts and data |
| `screenshots/04-hardcoded-key-discovery.png` | Discovery | Hardcoded cryptographic key located in client-side content |
| `screenshots/05-document-decryption.png` | Impact | Protected document decrypted after key discovery |

## OWASP Reference

https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/
