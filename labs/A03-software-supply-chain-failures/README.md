# A03:2025 — Software Supply Chain Failures

> Previously: A06:2021 — Vulnerable and Outdated Components (expanded scope in 2025)

## Description

Software supply chain failures cover risks from third-party code, compromised dependencies,
untrusted package sources, build pipeline tampering, and failure to maintain a software
bill of materials (SBOM). This is a broadened evolution of the 2021 "Vulnerable Components"
category, now explicitly including supply chain attack vectors.

## OWASP Reference

https://owasp.org/Top10/2025/A03_2025-Software_Supply_Chain_Failures/

## Key Concepts

- Known CVEs in third-party libraries
- Dependency confusion attacks
- Typosquatting in package registries
- Compromised upstream packages
- Unsigned or unverified packages
- Missing SBOM (Software Bill of Materials)
- Build pipeline and CI/CD compromise
- Unsupported or abandoned libraries

## Lab Platforms

- [x] TryHackMe
- [ ] PortSwigger

## Tools

- Dependency manifests (package.json, requirements.txt, pom.xml)
- `npm audit` / `pip-audit` / `osv-scanner`
- Public vulnerability databases (NVD, OSV, Snyk)
- SBOM generators (CycloneDX, SPDX)

## Workflow

1. Inventory all dependencies and their versions.
2. Check against public vulnerability databases.
3. Review package origins and signing status.
4. Validate build pipeline integrity.
5. Capture evidence in `screenshots/`.
6. Record findings and remediation.

## Checklist

- [x] Dependency manifest reviewed
- [x] Versions and CVEs checked
- [x] Package origins validated
- [ ] Build pipeline reviewed
- [x] Screenshots captured
- [x] Remediation written

## Notes

See `notes.md` for findings and methodology.

