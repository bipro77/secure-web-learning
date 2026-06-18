# Findings — Local Unverified Library Supply Chain Failure

## Summary

| # | Finding | Severity | Evidence |
| --- | --- | --- | --- |
| 1 | Unverified local library imported before trusted package paths | High | `sys.path.insert(0, .../lib)` |
| 2 | Missing dependency provenance and inventory controls | High | No manifest, lockfile, hash validation, or SBOM evidence |
| 3 | Public debug trigger exposes dependency internals | High | `data == "debug"` calls `debug_info()` |
| 4 | Flask debug mode enabled on exposed service | High | `app.run(..., debug=True)` |
| 5 | Sensitive lab data disclosed through vulnerable path | High | Flag retrieval screenshot |

---

## Finding 1 — Unverified Local Library Imported Before Trusted Package Paths

| Field | Value |
| --- | --- |
| Severity | High |
| Source | Source code review |
| OWASP 2025 | A03 — Software Supply Chain Failures |

### Evidence

```python
sys.path.insert(0, os.path.join(os.path.dirname(__file__), 'lib'))
from vulnerable_utils import process_data, format_output, debug_info
```

### Screenshot

`screenshots/02-source-code-review.png`

### Impact

- A local `lib` directory is prioritized before standard trusted package paths.
- If that directory is modified, replaced, or populated by an attacker-controlled file, the application imports malicious code under a trusted module name.
- The application has no visible package provenance, version pin, signature, or hash validation for the imported module.

### Root Cause

The application treats local third-party-style code as trusted runtime code without a managed dependency process. The import boundary is not protected by package management, artifact verification, or deployment controls.

---

## Finding 2 — Missing Dependency Provenance and Inventory Controls

| Field | Value |
| --- | --- |
| Severity | High |
| Source | Lab file and source review |
| OWASP 2025 | A03 — Software Supply Chain Failures |

### Evidence

No dependency control was visible for the reviewed local library:

- no pinned package manifest tied to the imported helper module
- no lockfile or deterministic build artifact
- no package hash verification
- no package signature or origin validation
- no SBOM for runtime dependency inventory

### Screenshot

`screenshots/03-vulnerability-identification.png`

### Impact

- The deployed code cannot be reliably reproduced.
- Defenders cannot quickly answer which dependency version is running.
- Vulnerability scanning and incident response are weakened because there is no complete dependency inventory.
- Malicious or outdated code can persist as a local helper without appearing in normal dependency review.

### Root Cause

The project lacks software supply chain controls around third-party or vendored code. Local code was trusted by location instead of verified by origin, version, and integrity.

---

## Finding 3 — Public Debug Trigger Exposes Dependency Internals

| Field | Value |
| --- | --- |
| Severity | High |
| Endpoint | `POST /api/process` |
| OWASP 2025 | A03 — Software Supply Chain Failures + A02 |

### Evidence

```python
if data == 'debug':
    return jsonify(debug_info())
```

### Screenshot

`screenshots/04-debug-endpoint-exploitation.png`

### Impact

- An unauthenticated user can trigger diagnostic behavior through normal API input.
- The API exposes information produced by the unverified dependency.
- Debug paths often reveal internal state, dependency behavior, version details, file paths, or sensitive lab data.

### Root Cause

Diagnostic helper functionality from the dependency was reachable from a public API route. Debug behavior was not restricted to authenticated administrative tooling or server-side logs.

---

## Finding 4 — Flask Debug Mode Enabled on Exposed Service

| Field | Value |
| --- | --- |
| Severity | High |
| Source | Source code review |
| OWASP 2025 | A03 + A02 |

### Evidence

```python
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
```

### Screenshot

`screenshots/01-recon-nmap-service-scan.png`

### Impact

- Debug mode can expose stack traces and source context when errors occur.
- Binding to `0.0.0.0` exposes the development server beyond localhost.
- Combined with a weak dependency boundary, debug mode increases the chance that dependency internals become visible to attackers.

### Root Cause

Development server settings were left active in the deployed lab service.

---

## Finding 5 — Sensitive Data Disclosed Through Vulnerable Dependency/Debug Path

| Field | Value |
| --- | --- |
| Severity | High |
| Evidence | Flag retrieval |
| OWASP 2025 | A03 — Software Supply Chain Failures |

### Evidence

The final lab step retrieved the flag after identifying and interacting with the vulnerable dependency/debug path. The actual flag value is intentionally not stored in the report.

### Screenshot

`screenshots/05-flag-retrieval.png`

### Impact

- Demonstrates that the supply chain weakness is exploitable, not just theoretical.
- Confirms that dependency diagnostics or behavior can disclose sensitive data.
- Shows why third-party code must be reviewed and constrained before exposure through application routes.

### Root Cause

An unverified dependency path and public debug behavior combined to expose sensitive lab data.
