# Methodology

Reusable documentation patterns for authorized OWASP Top 10, API security, and web application security practice.

The goal of this directory is consistency. Each lab should be understandable to a beginner, useful to a defender, and clean enough to present in a GitHub portfolio.

## Templates

| Template | Purpose |
| --- | --- |
| `templates/methodology.md` | Testing plan, scope, workflow, and evidence-handling rules |
| `templates/findings.md` | Finding structure, evidence summary, impact, and retest notes |
| `templates/mitigation.md` | Root cause, recommended controls, implementation notes, and validation |
| `templates/lessons-learned.md` | Reflection, defensive perspective, and follow-up study |
| `templates/lab-readme.md` | Standard landing page for a new lab writeup |
| `templates/notes.md` | General notes template for observations and evidence indexing |

## Documentation Rules

- Confirm scope before testing.
- Use placeholders for credentials, tokens, cookies, private IPs, emails, usernames, and resource identifiers.
- Keep reproduction notes concise, sanitized, and lab-specific.
- Do not include active exploit chains.
- Pair every finding with remediation and validation notes.
- Explain technical concepts in plain language before adding deeper details.

## Evidence Placeholders

Use these placeholders instead of sensitive values:

- `<TARGET_HOST>`
- `<RESOURCE_ID>`
- `<USER_ID>`
- `<SESSION_COOKIE>`
- `<TOKEN>`
- `<API_KEY>`
- `<REDACTED>`
