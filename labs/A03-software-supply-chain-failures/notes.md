# Notes: A03 Software Supply Chain Failures

## Lab Metadata

- Date: 2025-06-17
- Platform: TryHackMe
- Lab name: Local Unverified Library Supply Chain Failure
- Difficulty: Beginner
- Status: Completed

## Summary

The target Flask application imported a local third-party-style module by prepending a
relative `lib` directory to `sys.path`. The source showed that core processing logic
was delegated to `vulnerable_utils` without package provenance, version pinning, hash
verification, signing, or an SBOM. A debug trigger exposed dependency diagnostic output
through `/api/process`, and the application ran with Flask debug mode enabled.

## Key Findings

| Finding | Evidence | Impact |
| --- | --- | --- |
| Local unverified library imported before trusted paths | `sys.path.insert(0, .../lib)` in app source | Dependency hijacking and malicious local package risk |
| No package manifest or lockfile evidence | Source review and lab files | No repeatable dependency inventory or provenance trail |
| Debug branch exposes dependency internals | `data == "debug"` calls `debug_info()` | Internal implementation details and sensitive lab data exposed |
| Flask debug mode enabled | `app.run(..., debug=True)` | Stack traces and interactive debugger risk in exposed service |
| Flag retrieved through vulnerable debug/dependency path | Screenshot evidence | Demonstrates practical data disclosure from supply chain weakness |

## Evidence

- Screenshots: `tryhackme/screenshots/01` through `05`
- Source artifact: `tryhackme/app-1763304148639.py`
- Full writeup: `tryhackme/README.md`

## Lessons Learned

- Treat local libraries and vendored code as supply chain inputs, not trusted application code by default.
- Dependency provenance matters as much as dependency version.
- Debug helpers inside dependencies create production exposure when reachable from API routes.
- A03 often overlaps with A02 when insecure deployment settings expose the weak dependency path.
