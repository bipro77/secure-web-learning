# Mitigation - Client-Side Key Exposure and Document Decryption

## Overview

The vulnerability is a cryptographic failure because the application exposes both protected data and the key material needed to decrypt it to an untrusted browser. The fix is to move secrets and authorization decisions to trusted server-side components.

---

## Fix 1 - Remove Hardcoded Client-Side Keys

**Problem:** The browser can inspect and recover hardcoded keys from source, static assets, or runtime variables.

**Fix:** Never embed encryption keys, passphrases, or signing secrets in client-side code.

Use server-side secret storage instead:

```text
Application server -> Secret manager / KMS -> Decrypt or authorize server-side
Browser -> Receives only data the user is authorized to access
```

Recommended controls:

- store keys in a managed secret store or KMS
- restrict key access by service identity
- audit key usage
- rotate keys on a defined schedule and after suspected exposure

---

## Fix 2 - Enforce Authorization Before Data Release

**Problem:** The client received the encrypted document and the means to decrypt it.

**Fix:** Require server-side authorization before returning protected content.

```text
1. Authenticate requester.
2. Check authorization for the requested document.
3. Decrypt server-side only if access is allowed, or issue a short-lived authorized response.
4. Return the minimum necessary data.
```

Client-side checks can improve usability, but they must not be the security boundary.

---

## Fix 3 - Keep Decryption in Trusted Components

Move decryption operations to trusted backend services or managed cryptographic services.

Avoid patterns where the browser receives:

- ciphertext
- decryption key
- algorithm parameters
- reusable decryption logic

If client-side cryptography is required for a legitimate end-to-end encryption design, keys must be derived from user-controlled secrets that the server cannot disclose, and the threat model must explicitly account for malicious clients.

---

## Fix 4 - Use Modern Cryptographic APIs Correctly

Use authenticated encryption and strong key generation:

```text
Algorithm: AES-GCM or ChaCha20-Poly1305
Key source: CSPRNG or managed KMS
Nonce/IV: unique per encryption operation
Authentication: verify tags before using plaintext
```

Avoid:

- custom cryptography
- reused IVs/nonces
- static shared keys
- deprecated algorithms such as DES, RC4, MD5, or SHA1 for security-sensitive purposes

---

## Fix 5 - Add Secret Scanning and Build Controls

Add automated checks so exposed keys are caught before deployment:

```bash
gitleaks detect
trufflehog filesystem .
```

Recommended controls:

- block commits containing likely secrets
- scan static assets and bundled JavaScript
- review generated frontend bundles for accidental secret exposure
- keep production configuration separate from source code

---

## Verification Steps

1. Search client-delivered HTML and JavaScript for keys, secrets, and passphrases.
2. Confirm protected documents are not returned before server-side authorization.
3. Confirm decryption keys are not visible in page source, developer tools, local storage, session storage, or network responses.
4. Attempt the previous decryption workflow and verify it no longer works from browser-only material.
5. Rotate any key that was previously exposed.
6. Confirm secret scanning runs in development and CI.
