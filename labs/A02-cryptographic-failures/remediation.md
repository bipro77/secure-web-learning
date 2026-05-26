# Remediation: A02 Cryptographic Failures

## Root Cause

Document where sensitive data protection failed.

## Recommended Fixes

- Use modern TLS configurations.
- Store passwords with Argon2id, bcrypt, or scrypt using unique salts.
- Protect secrets with a managed secrets vault.
- Disable weak cryptographic algorithms and protocols.
- Avoid exposing sensitive data in URLs, logs, errors, or client-side code.

## Verification

1. Confirm sensitive values are not exposed.
2. Validate encryption in transit and at rest.
3. Re-test token, cookie, and secret handling.
4. Add checks for sensitive data leakage.

