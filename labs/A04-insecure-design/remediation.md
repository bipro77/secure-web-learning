# Remediation: A04 Insecure Design

## Root Cause

Document the design assumption, missing control, or error-handling decision that allowed enumeration, debug disclosure, or excessive error detail.

## Recommended Fixes

- Add threat modeling to design work.
- Define abuse cases for important workflows.
- Enforce business rules server-side.
- Add rate limits, replay protection, and state validation where appropriate.
- Review trust boundaries before implementation.
- Return consistent, generic client-facing error messages.
- Keep detailed diagnostics in protected server-side logs only.
- Disable debug mode in production-like environments.
- Normalize API responses so invalid routes, methods, and IDs do not reveal unnecessary implementation details.

## Verification

1. Re-test the abused workflow.
2. Confirm invalid states are rejected.
3. Confirm malformed API requests no longer expose stack traces, framework details, file paths, or debug fields.
4. Confirm normal business flow still works.
5. Add tests for abuse cases and exception-handling paths.
