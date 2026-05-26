# A08:2025 — Software or Data Integrity Failures

> Previously: A08:2021 — Software and Data Integrity Failures (expanded name)

## Description

Failures where code, updates, CI/CD pipelines, or serialized/deserialized data are
trusted without integrity verification, allowing attackers to insert malicious code,
tamper with data, or hijack automated pipelines.

## OWASP Reference

https://owasp.org/Top10/2025/A08_2025-Software_or_Data_Integrity_Failures/

## Key Concepts

- Insecure deserialization (Java, PHP, Python pickle, etc.)
- Unsigned or unverified software updates
- CI/CD pipeline poisoning
- Dependency integrity (SRI for CDN assets, package checksums)
- Tampered serialized objects or JWTs
- Auto-update mechanisms without signature verification

## Lab Platforms

- [ ] TryHackMe
- [ ] PortSwigger

## Tools

- Browser developer tools
- Burp Suite
- Dependency manifests with hash verification
- Serialization tooling (authorized labs only)
- SRI hash generators

## Workflow

1. Identify trusted data sources, update flows, and pipeline inputs.
2. Review signature, hash, and verification mechanisms.
3. Test object tampering in authorized lab conditions.
4. Check CDN resources for missing Subresource Integrity (SRI).
5. Capture evidence in `screenshots/`.
6. Record findings and remediation.

## Checklist

- [ ] Integrity boundaries identified
- [ ] Trusted data flows mapped
- [ ] Deserialization surfaces reviewed
- [ ] CI/CD pipeline trust reviewed
- [ ] Tampering tested in authorized scope
- [ ] Screenshots captured
- [ ] Remediation written

## Notes

See `notes.md` for findings and methodology.

