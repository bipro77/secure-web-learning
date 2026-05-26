# Remediation: A10 Server-Side Request Forgery (SSRF)

## Root Cause

Document how user-controlled input influenced server-side requests.

## Recommended Fixes

- Avoid direct user-controlled server-side fetches where possible.
- Enforce strict allowlists for schemes, hosts, and ports.
- Resolve and validate DNS and IP ranges before requests.
- Block access to loopback, link-local, private, and metadata addresses.
- Segment networks so application servers cannot reach unnecessary internal services.

## Verification

1. Re-test the original SSRF payload.
2. Confirm disallowed destinations are blocked.
3. Confirm approved destinations still work.
4. Add tests for URL parser bypass cases.

