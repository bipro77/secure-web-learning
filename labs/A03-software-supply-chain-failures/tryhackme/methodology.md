# Methodology — Local Unverified Library Supply Chain Failure

## Target

| Field | Value |
| --- | --- |
| Platform | TryHackMe |
| Primary service | Flask API |
| Primary port | 5000 |
| Authorization | TryHackMe lab — authorized testing only |
| Evidence | Browser, nmap, source review, API requests |

---

## Phase 1 — Reconnaissance

### 1.1 Service Scan

Run a service and version scan against the assigned target:

```bash
nmap -sC -sV <TARGET_IP>
```

Record the exposed HTTP service, port, framework hints, and whether any development server banners are visible.

### Screenshot

`screenshots/01-recon-nmap-service-scan.png`

---

## Phase 2 — Source Code Review

### 2.1 Identify dependency boundaries

Review the application source for imports, local libraries, vendored code, and runtime path manipulation.

Key evidence from this lab:

```python
sys.path.insert(0, os.path.join(os.path.dirname(__file__), 'lib'))
from vulnerable_utils import process_data, format_output, debug_info
```

### What to look for

- local `lib`, `vendor`, or `third_party` directories
- imports from files that are not managed by a package manifest
- `sys.path.insert()` or `PYTHONPATH` changes
- missing lockfiles, hashes, signatures, or SBOMs
- debug helper functions exposed by dependency code

### Screenshot

`screenshots/02-source-code-review.png`

---

## Phase 3 — Vulnerability Identification

### 3.1 Classify the supply chain risk

Map the observed behavior to A03:

- the application trusts unverified local helper code
- dependency origin and integrity cannot be proven
- runtime import order can be influenced by local files
- dependency diagnostic output is reachable from application code

### Screenshot

`screenshots/03-vulnerability-identification.png`

---

## Phase 4 — Controlled API Testing

### 4.1 Baseline request

Send normal input to the processing endpoint:

```http
POST /api/process HTTP/1.1
Host: <TARGET_IP>:5000
Content-Type: application/json

{"data":"test"}
```

Confirm the API processes data through the imported dependency.

### 4.2 Debug trigger

Send the debug trigger identified in source review:

```http
POST /api/process HTTP/1.1
Host: <TARGET_IP>:5000
Content-Type: application/json

{"data":"debug"}
```

Observe whether dependency diagnostic output is returned to the client.

### Screenshot

`screenshots/04-debug-endpoint-exploitation.png`

---

## Phase 5 — Impact Confirmation

Confirm the lab objective without storing sensitive values in the repository.

Evidence handling rules:

- screenshot the flag retrieval step
- redact or avoid committing raw flag values in Markdown
- document the vulnerable path, not reusable exploitation against real systems

### Screenshot

`screenshots/05-flag-retrieval.png`
