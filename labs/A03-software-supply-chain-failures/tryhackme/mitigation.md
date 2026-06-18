# Mitigation — Local Unverified Library Supply Chain Failure

## Overview

The vulnerability is a software supply chain failure because application behavior depends on local helper code whose origin, version, and integrity are not verifiable. The fix is to make dependencies explicit, reproducible, scanned, and constrained at runtime.

---

## Fix 1 — Remove Unverified Import Path Precedence

**Problem:** The application places a local `lib` directory at the front of the import path:

```python
sys.path.insert(0, os.path.join(os.path.dirname(__file__), 'lib'))
```

**Fix:** Convert the helper code into a reviewed internal package or install a verified external package through the standard dependency manager.

```python
from trusted_processing import process_data, format_output
```

Avoid relying on local directory precedence for production imports.

---

## Fix 2 — Pin Dependencies and Verify Integrity

Use pinned versions and hashes so builds are deterministic:

```text
Flask==3.1.0 --hash=sha256:<EXPECTED_HASH>
trusted-processing==1.4.2 --hash=sha256:<EXPECTED_HASH>
```

Recommended controls:

- generate lockfiles in CI
- use hash-checking mode for Python installs
- restrict package sources to approved internal or public registries
- fail builds when dependencies are unpinned

---

## Fix 3 — Maintain an SBOM

Generate a software bill of materials for every build:

```bash
cyclonedx-py environment --output-format json --output-file sbom.json
```

Use the SBOM to support vulnerability management, incident response, and dependency ownership reviews.

---

## Fix 4 — Remove Public Debug Triggers

**Problem:** The API returns dependency diagnostic output when user input equals `debug`.

```python
if data == 'debug':
    return jsonify(debug_info())
```

**Fix:** Remove this branch from public routes. Diagnostics should be server-side only, or protected behind authenticated administrative tooling with audit logging.

---

## Fix 5 — Disable Flask Debug Mode

**Problem:** The service starts with debug mode enabled and listens on all interfaces.

```python
app.run(host='0.0.0.0', port=5000, debug=True)
```

**Fix:**

```bash
FLASK_ENV=production
FLASK_DEBUG=0
gunicorn --bind 0.0.0.0:5000 --workers 4 app:app
```

Add generic error handling:

```python
@app.errorhandler(Exception)
def handle_exception(e):
    app.logger.error("Unhandled exception", exc_info=True)
    return jsonify({"error": "An internal error occurred."}), 500
```

---

## Verification Steps

1. Confirm application imports do not depend on `sys.path.insert()` for local libraries.
2. Confirm dependencies are pinned and installed from approved sources.
3. Confirm package hashes or signatures are checked in CI.
4. Confirm an SBOM exists for the deployed artifact.
5. Send `{"data":"debug"}` and verify no debug output is returned.
6. Trigger an application error and verify no stack trace or dependency internals are returned.
7. Run dependency scanners such as `pip-audit` or `osv-scanner` and triage findings.
