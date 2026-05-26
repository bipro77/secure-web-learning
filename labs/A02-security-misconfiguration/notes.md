# Notes: A02 Security Misconfiguration

## Lab Metadata

- Date: 2025-05-25
- Platform: TryHackMe
- Lab name: User Management API Misconfiguration
- Difficulty: Beginner
- Status: Completed

## Summary

The target application exposed a User Management API on ports 5002, 5003, and 5004
running Werkzeug/3.1.3 (Python/3.11.14) in debug mode. No authentication was enforced
on any user data endpoint. Input validation was absent, allowing boundary and type
confusion requests to trigger full stack traces in HTTP responses. The `Server` header
disclosed the exact framework and runtime version on every response.

## Key Findings

| Finding                              | Endpoint              | Impact                                |
|--------------------------------------|-----------------------|---------------------------------------|
| Server header discloses stack        | All responses         | Fingerprinting — framework + version  |
| No authentication on user endpoints  | `/api/user/<id>`      | Any user data accessible unauthenticated |
| User enumeration via sequential IDs  | `/api/user/1`, `/123` | Full user record returned with no auth |
| Out-of-range ID — no error handling  | `/api/user/999999`    | Verbose error or unexpected response  |
| Negative ID — no input validation    | `/api/user/-1`        | Unhandled boundary — error disclosure |
| Type confusion — full stack trace    | `/api/user/xyz`       | Werkzeug debug trace with file paths  |

## Observations

- Ports 5002, 5003, 5004 all ran the same Werkzeug stack — three separate services exposed
- `nmap -sC -sV` immediately revealed the framework and Python version via the Server header
- Valid IDs (1, 123) returned full user records with no token or session required
- `/api/user/xyz` returned a Werkzeug interactive debug page — confirming `DEBUG=True` in production
- The debug page included internal file paths, source code lines, and Python runtime details

## Evidence

- Screenshots: `screenshots/01` through `05` (category level)
- Lab writeup: `tryhackme/user-management-api-misconfiguration/`

## Lessons Learned

- Debug mode in production is a critical misconfiguration — not a code bug
- Verbose errors are the symptom; missing error handlers + debug mode enabled is the cause
- API endpoints need authentication enforced at the route level, not assumed
- Server headers should always be suppressed or replaced before production deployment
- See full lessons-learned: `tryhackme/user-management-api-misconfiguration/lessons-learned.md`

