# Remediation: A03 Software Supply Chain Failures

## Root Cause Summary

The application trusted local third-party-style code without a verifiable supply chain.
The app inserted a local `lib` directory at the front of Python's import path and then
imported processing and debug helpers from `vulnerable_utils`. No dependency manifest,
lockfile, hash pinning, signature validation, SBOM, or package origin record was present.
The exposed debug path and Flask debug mode made the weak dependency boundary observable
from the web interface.

## Findings and Fixes

| Finding | Priority | Fix |
| --- | --- | --- |
| Unverified local library import | High | Package the library, pin versions, verify hashes, and avoid import path prepending |
| Missing dependency inventory | High | Add `requirements.txt` or `pyproject.toml` plus a lockfile and SBOM |
| Debug helper exposed through API | High | Remove public debug triggers and restrict diagnostics to authenticated admin tooling |
| Flask debug mode enabled | High | Set `FLASK_DEBUG=0` and run behind a production WSGI server |
| Verbose exception output | Medium | Return generic errors to clients and log full details server-side |

## Recommended Fixes

### 1. Remove import path manipulation

```python
# Avoid this pattern in production code:
# sys.path.insert(0, os.path.join(os.path.dirname(__file__), 'lib'))

from trusted_package.processing import process_data, format_output
```

### 2. Pin and verify dependencies

```text
# requirements.txt
trusted-package==1.4.2 --hash=sha256:<EXPECTED_HASH>
Flask==3.1.0 --hash=sha256:<EXPECTED_HASH>
```

Use a deterministic build flow such as `pip-compile --generate-hashes`, Poetry lockfiles,
or another approved dependency management process.

### 3. Generate and maintain an SBOM

```bash
cyclonedx-py environment --output-format json --output-file sbom.json
```

Review the SBOM in CI and store it with build artifacts so deployed code can be traced
back to exact package versions and origins.

### 4. Remove public debug behavior

```python
@app.route('/api/process', methods=['POST'])
def process():
    data = request.json.get('data', '')
    if not data:
        return jsonify({'error': 'Missing data parameter'}), 400

    processed = process_data(data)
    return jsonify({'result': format_output(processed), 'status': 'success'})
```

### 5. Disable debug mode and add generic errors

```python
app.config['DEBUG'] = False

@app.errorhandler(Exception)
def handle_exception(e):
    app.logger.error("Unhandled exception", exc_info=True)
    return jsonify({"error": "An internal error occurred."}), 500
```

## Verification Steps

1. Confirm `sys.path.insert()` is not used to prioritize unverified local code.
2. Confirm dependency versions and hashes are pinned in the build process.
3. Generate an SBOM and verify all runtime packages are represented.
4. Send `{"data":"debug"}` to `/api/process` and confirm no diagnostic output is returned.
5. Confirm `FLASK_DEBUG=0` and no Werkzeug debugger appears on errors.
6. Run dependency scanning in CI and fail builds for critical vulnerable packages.
