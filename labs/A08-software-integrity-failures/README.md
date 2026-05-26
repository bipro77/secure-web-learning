# A08: Software Integrity Failures

## Objective

Study failures where code, updates, CI/CD processes, or serialized data are trusted without integrity checks.

## Scope

- Platform:
- Lab or room:
- Target URL:
- Integrity boundary:

## Key Concepts

- Insecure deserialization
- Unsigned updates
- CI/CD pipeline trust
- Dependency integrity
- Tampered data or objects

## Tools

- Browser developer tools
- Burp Suite
- Dependency manifests
- Serialization tooling when authorized

## Workflow

1. Identify trusted data and update flows.
2. Review signatures, hashes, and verification behavior.
3. Test tampering in authorized lab conditions.
4. Capture evidence in `screenshots/`.
5. Record findings and remediation.

## Checklist

- [ ] Integrity boundary identified
- [ ] Trusted data flow mapped
- [ ] Tampering tested
- [ ] Screenshots captured
- [ ] Remediation written

