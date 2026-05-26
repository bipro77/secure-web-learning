# A04: Insecure Design

## Objective

Analyze design-level security gaps that appear when an application workflow, API behavior, or error-handling model exposes more information than users need.

## Scope

- Platform:
- Lab or room:
- Target URL:
- Business workflow:

## Key Concepts

- Missing threat modeling
- Unsafe business logic
- Weak abuse-case handling
- Missing rate limits or workflow constraints
- Trust boundary mistakes
- API enumeration through predictable endpoints or response differences
- Verbose error leakage and debug information disclosure
- Improper exception handling

## Tools

- Threat modeling notes
- Sequence diagrams
- Browser developer tools
- Burp Suite
- curl

## Workflow

1. Document the intended workflow.
2. Identify assumptions and trust boundaries.
3. Test alternate paths, API methods, malformed inputs, and abuse cases.
4. Capture evidence in `screenshots/`.
5. Record findings in `notes.md`.
6. Document design-level mitigations.

## Checklist

- [ ] Workflow mapped
- [ ] Trust boundaries identified
- [ ] Abuse cases tested
- [ ] Screenshots captured
- [ ] Remediation written
