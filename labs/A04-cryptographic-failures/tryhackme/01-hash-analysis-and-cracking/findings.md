# Findings - Client-Side Key Exposure and Document Decryption

## Summary

| # | Finding | Severity | Evidence |
| --- | --- | --- | --- |
| 1 | Hardcoded cryptographic key exposed to the browser | High | `screenshots/04-hardcoded-key-discovery.png` |
| 2 | Client-side decryption protects data only cosmetically | High | `screenshots/02-page-source.png`, `screenshots/03-developer-tools.png` |
| 3 | Sensitive encrypted document is retrievable by the client | High | `screenshots/05-document-decryption.png` |
| 4 | Missing server-side authorization for protected content | High | Document decrypted through browser-accessible workflow |
| 5 | No visible key management or rotation controls | Medium | Key appears embedded in static/client-side material |

---

## Finding 1 - Hardcoded Cryptographic Key Exposed to the Browser

| Field | Value |
| --- | --- |
| Severity | High |
| Source | Page source and developer tools review |
| OWASP 2025 | A04 - Cryptographic Failures |

### Evidence

The lab evidence shows a cryptographic key discovered in browser-accessible content.

### Screenshot

`screenshots/04-hardcoded-key-discovery.png`

### Impact

- Any user who can load the page can inspect the source and recover the key.
- The key cannot be treated as a secret once it is shipped to the browser.
- If the same key protects multiple documents or users, one disclosure can expose all data encrypted with that key.
- Rotation is difficult because exposed client code may be cached, copied, or reused.

### Root Cause

The application stores secret cryptographic material in client-side code or client-delivered assets. Browser code is not a trusted storage location for encryption keys.

---

## Finding 2 - Client-Side Decryption Protects Data Only Cosmetically

| Field | Value |
| --- | --- |
| Severity | High |
| Source | Browser developer tools |
| OWASP 2025 | A04 - Cryptographic Failures |

### Evidence

The screenshot sequence shows review of page source and developer tools before the key and decrypted document were obtained.

### Screenshot

`screenshots/02-page-source.png`

`screenshots/03-developer-tools.png`

### Impact

- Encryption does not provide meaningful access control when the ciphertext, key, and decryption logic are all delivered to the same untrusted client.
- A user can reproduce the decryption workflow outside the intended UI.
- The application may create a false sense of security because the document appears encrypted at rest, but the decryption boundary is attacker-controlled.

### Root Cause

Cryptography was used as a substitute for server-side authorization. The server should decide whether the requester may access the plaintext before releasing sensitive content.

---

## Finding 3 - Sensitive Document Decrypted After Key Discovery

| Field | Value |
| --- | --- |
| Severity | High |
| Evidence | Document decryption screenshot |
| OWASP 2025 | A04 - Cryptographic Failures |

### Evidence

The final evidence screenshot shows successful document decryption after identifying the hardcoded key. Sensitive lab values are intentionally not copied into this report.

### Screenshot

`screenshots/05-document-decryption.png`

### Impact

- Confirms that the exposed key is exploitable, not merely an information leak.
- Demonstrates loss of confidentiality for the protected document.
- Shows that browser inspection alone was sufficient to recover protected content.

### Root Cause

The protected document could be accessed and decrypted by the client because key secrecy and data authorization were enforced in the browser instead of on the server.

---

## Finding 4 - Missing Server-Side Authorization for Protected Content

| Field | Value |
| --- | --- |
| Severity | High |
| Source | Application behavior |
| OWASP 2025 | A04 + A01 |

### Evidence

The application allowed the client to obtain the encrypted content and the material needed to decrypt it.

### Screenshot

`screenshots/05-document-decryption.png`

### Impact

- Users can bypass intended UI restrictions by inspecting or replaying browser requests.
- Sensitive data exposure is likely if authorization is not checked before serving protected data.
- Client-side controls can be modified, disabled, or replayed by the user.

### Root Cause

The security boundary was placed in browser-side logic. Authorization decisions must be made server-side and enforced before sensitive data or decryption capability is released.

---

## Finding 5 - Missing Key Management and Rotation Controls

| Field | Value |
| --- | --- |
| Severity | Medium |
| Source | Client-side key discovery |
| OWASP 2025 | A04 - Cryptographic Failures |

### Evidence

The key appeared in static or browser-delivered material, with no visible separation between application code and secret material.

### Screenshot

`screenshots/04-hardcoded-key-discovery.png`

### Impact

- Secrets in source or static assets can leak through browser inspection, code repositories, logs, caches, and backups.
- There is no practical way to guarantee that all exposed copies have been removed after disclosure.
- Long-lived shared keys increase the blast radius of a single exposure.

### Root Cause

The application lacks a server-side secret management pattern. Keys should be generated, stored, rotated, and used in trusted server-side or managed cryptographic services.
