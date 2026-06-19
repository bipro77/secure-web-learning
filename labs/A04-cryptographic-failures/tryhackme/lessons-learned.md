# Lessons Learned - Client-Side Key Exposure and Document Decryption

## What Was Found

| Finding | Severity |
| --- | --- |
| Hardcoded cryptographic key exposed to the browser | High |
| Decryption logic available in client-side code | High |
| Sensitive document decrypted after source inspection | High |
| Server-side authorization did not protect decrypted content | High |
| Key management and rotation controls were missing | Medium |

---

## 1. Browser-Delivered Secrets Are Public

Anything sent to the browser can be read, copied, modified, replayed, or debugged by the user. Obfuscation, minification, or hiding a key in a JavaScript variable does not make it secret.

## 2. Encryption Is Not Authorization

Encrypting a document does not control access if every client receives the ciphertext and the decryption key. Authorization must happen before the server releases sensitive data or decryption capability.

## 3. Client-Side Cryptography Needs a Clear Threat Model

Client-side cryptography can be valid in designs such as end-to-end encryption, but only when keys are not provided by the server to unauthorized users. In this lab, the client had enough information to recover the protected document, so the cryptographic boundary failed.

## 4. Developer Tools Are Part of the Test Surface

Browser developer tools expose loaded scripts, network responses, storage, runtime variables, and breakpoints. Any web cryptography review should include source inspection and runtime inspection.

## 5. Checklist for Future Cryptographic Failure Labs

```text
Recon
[ ] Identify sensitive data and protected documents
[ ] Identify where ciphertext, keys, tokens, and secrets are stored

Client Review
[ ] Search page source for key, secret, token, password, decrypt, and crypto terms
[ ] Inspect bundled JavaScript and comments
[ ] Inspect local storage, session storage, cookies, and network responses

Crypto Review
[ ] Confirm keys are not hardcoded or client-delivered
[ ] Confirm modern authenticated encryption is used
[ ] Confirm IVs/nonces are unique where required
[ ] Confirm deprecated algorithms are not used

Authorization Review
[ ] Confirm server-side authorization occurs before sensitive data is returned
[ ] Confirm client-side checks are not the only access control
[ ] Confirm previous browser-only decryption paths no longer work
```

---

## Follow-Up Study

| Resource | URL |
| --- | --- |
| OWASP A04:2025 Cryptographic Failures | https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures/ |
| OWASP Cryptographic Storage Cheat Sheet | https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html |
| OWASP Secrets Management Cheat Sheet | https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html |
| OWASP Transport Layer Security Cheat Sheet | https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html |
| NIST Key Management Guidelines | https://csrc.nist.gov/projects/key-management |
