# Remediation: A04 Cryptographic Failures

## Root Cause

The application exposes cryptographic key material and decryption logic to the browser. Because the client receives both the protected data and the material needed to decrypt it, encryption no longer provides confidentiality.

## Recommended Fixes

- Remove hardcoded keys, passphrases, and secrets from client-side code.
- Store key material in a server-side secret manager or key management service.
- Enforce authorization on the server before returning protected documents.
- Perform decryption in trusted backend components unless a documented end-to-end encryption design requires otherwise.
- Use modern authenticated encryption such as AES-GCM or ChaCha20-Poly1305.
- Generate keys with a cryptographically secure random source.
- Use unique nonces or IVs where required by the algorithm.
- Rotate any key that has been exposed to the browser.
- Add secret scanning for source code, generated frontend bundles, and deployment artifacts.

## Verification

1. Search page source and bundled JavaScript for keys, secrets, passphrases, and decryption material.
2. Confirm protected documents are not returned before server-side authorization succeeds.
3. Confirm browser developer tools no longer reveal reusable decryption keys.
4. Attempt the previous document decryption path and verify browser-only material is insufficient.
5. Confirm secret scanning runs locally and in CI.
