# Lab: Local Unverified Library Supply Chain Failure

## Classification

| Field | Value |
| --- | --- |
| OWASP 2025 | A03 — Software Supply Chain Failures |
| Platform | TryHackMe |
| Primary port | 5000 |
| Difficulty | Beginner |
| Status | Completed |

## Objective

Identify how an application can become vulnerable when it trusts local third-party-style
code without provenance checks, dependency inventory, or production-safe configuration.

## Vulnerability Class

- Unverified local dependency imported into application runtime
- Import path manipulation that prioritizes local code
- Missing dependency manifest, lockfile, hash pinning, and SBOM
- Public debug functionality exposed through an API endpoint
- Development/debug settings left enabled on an exposed Flask service

## Files

| File | Purpose |
| --- | --- |
| `methodology.md` | Step-by-step testing approach |
| `findings.md` | Discovered vulnerabilities |
| `mitigation.md` | How to fix each finding |
| `lessons-learned.md` | Key takeaways |
| `app-1763304148639.py` | Source artifact reviewed during the lab |
| `screenshots/` | Evidence captured during testing |

## Screenshots

| File | Phase | Finding |
| --- | --- | --- |
| `screenshots/01-recon-nmap-service-scan.png` | Recon | Service discovery and exposed Flask application |
| `screenshots/02-source-code-review.png` | Review | Source shows local unverified library import path |
| `screenshots/03-vulnerability-identification.png` | Analysis | Vulnerable supply chain pattern identified |
| `screenshots/04-debug-endpoint-exploitation.png` | Exploit | Debug behavior exposed through API processing path |
| `screenshots/05-flag-retrieval.png` | Impact | Flag retrieved through vulnerable dependency/debug path |

## OWASP Reference

https://owasp.org/Top10/2025/A03_2025-Software_Supply_Chain_Failures/
