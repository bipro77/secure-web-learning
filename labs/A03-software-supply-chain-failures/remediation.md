# Remediation: A03 Injection

## Root Cause

Document which interpreter received untrusted input and why input changed execution.

## Recommended Fixes

- Use parameterized queries or prepared statements.
- Avoid building commands or queries with string concatenation.
- Validate input using allowlists.
- Encode output in the correct context.
- Use safe framework APIs and sandbox risky execution paths.

## Verification

1. Re-run the original payloads.
2. Confirm the payloads are treated as data.
3. Validate logs and error responses do not expose internals.
4. Add automated tests for the affected input path.

