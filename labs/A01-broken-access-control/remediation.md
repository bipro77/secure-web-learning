# Remediation: A01 Broken Access Control

## Root Cause

Document why authorization failed.

## Recommended Fixes

- Enforce server-side authorization on every protected action.
- Deny access by default.
- Validate ownership and role requirements on each request.
- Avoid relying on hidden UI elements as access control.
- Log and alert on repeated authorization failures.

## Verification

1. Re-run the original exploit path.
2. Confirm unauthorized requests are denied.
3. Confirm legitimate users retain expected access.
4. Add regression tests for the affected authorization boundary.

