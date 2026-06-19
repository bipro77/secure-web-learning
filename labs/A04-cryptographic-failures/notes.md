# Notes: A04 Cryptographic Failures

## Lab Metadata

- Date: 2026-06-19
- Platform: TryHackMe
- Lab name: Client-side key exposure and document decryption
- Difficulty: Beginner
- Status: Completed

## Summary

The lab demonstrates a cryptographic failure where browser-accessible source and developer tools reveal the material needed to decrypt a protected document. The core issue is that secret key material and decryption capability are delivered to an untrusted client.

## Methodology

1. Load the web application and identify the protected document workflow.
2. Review page source for scripts, comments, and cryptographic terms.
3. Use browser developer tools to inspect loaded assets, runtime logic, and network responses.
4. Locate hardcoded key material in client-delivered content.
5. Confirm impact by decrypting the protected document without storing sensitive lab values in this repository.

## Findings

| Finding | Evidence | Impact |
| --- | --- | --- |
| Hardcoded cryptographic key exposed to the browser | `tryhackme/screenshots/04-hardcoded-key-discovery.png` | Any user who can load the page can recover the key |
| Client-side decryption controls protected content | `tryhackme/screenshots/02-page-source.png`, `tryhackme/screenshots/03-developer-tools.png` | Encryption does not provide meaningful access control |
| Protected document decrypted after key discovery | `tryhackme/screenshots/05-document-decryption.png` | Confidentiality of the document is lost |

## Evidence

- `tryhackme/screenshots/01-homepage.png`
- `tryhackme/screenshots/02-page-source.png`
- `tryhackme/screenshots/03-developer-tools.png`
- `tryhackme/screenshots/04-hardcoded-key-discovery.png`
- `tryhackme/screenshots/05-document-decryption.png`

## Lessons Learned

- Secrets in browser-delivered code are public.
- Encryption cannot replace server-side authorization.
- Decryption keys must be managed in trusted server-side components or dedicated key management systems.
