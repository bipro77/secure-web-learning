# Methodology - Client-Side Key Exposure and Document Decryption

## Target

| Field | Value |
| --- | --- |
| Platform | TryHackMe |
| Primary service | Web application |
| Authorization | TryHackMe lab - authorized testing only |
| Evidence | Browser, page source, developer tools, document decryption result |

---

## Phase 1 - Application Reconnaissance

### 1.1 Load the application

Open the assigned TryHackMe target in a browser and identify the main workflow, visible document controls, and any references to encrypted or protected content.

### Screenshot

`screenshots/01-homepage.png`

---

## Phase 2 - Page Source Review

### 2.1 Inspect delivered client-side content

Use the browser's view-source feature to review HTML, JavaScript references, inline scripts, comments, and static asset names.

Look for:

- inline cryptographic keys or passphrases
- JavaScript variables with names such as `key`, `secret`, `password`, or `token`
- crypto libraries and decryption helper functions
- encoded or encrypted document blobs
- comments that expose implementation details

### Screenshot

`screenshots/02-page-source.png`

---

## Phase 3 - Developer Tools Analysis

### 3.1 Inspect runtime scripts and requests

Open browser developer tools and review:

- loaded JavaScript files
- network requests for document or key material
- local storage, session storage, and cookies
- runtime variables related to cryptography
- decryption functions called by UI actions

### Screenshot

`screenshots/03-developer-tools.png`

---

## Phase 4 - Hardcoded Key Discovery

### 4.1 Locate exposed key material

Search the delivered source and runtime assets for hardcoded cryptographic material. Treat anything delivered to the browser as public from a security perspective.

Evidence handling rules:

- document where the key was found
- do not commit real secrets or lab flags into Markdown
- capture a screenshot showing the discovery location
- avoid reusing the key outside the authorized lab environment

### Screenshot

`screenshots/04-hardcoded-key-discovery.png`

---

## Phase 5 - Controlled Impact Confirmation

### 5.1 Decrypt the protected document

Use the application workflow or browser-accessible decryption logic to confirm that the exposed key can decrypt the protected document.

Record the impact without storing sensitive values in the repository.

### Screenshot

`screenshots/05-document-decryption.png`
