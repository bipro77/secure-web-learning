# Findings: API Enumeration and Verbose Error Leakage

## Overview

| Field          | Value                                           |
|----------------|-------------------------------------------------|
| OWASP 2025     | A02 — Security Misconfiguration                 |
| Platform       | TryHackMe                                       |
| Target port    | 5002                                            |
| Finding types  | API enumeration, verbose error leakage, stack trace disclosure |
| Status         | Completed                                       |

---

## Finding 1 — API User Enumeration via Sequential IDs

| Field      | Value                                        |
|------------|----------------------------------------------|
| Severity   | 🔴 High                                      |
| Endpoint   | `GET /api/user/<id>`                         |
| Type       | API enumeration + unauthenticated access     |
| OWASP 2025 | A02 Security Misconfiguration                |

### Description

The `/api/user/<id>` endpoint accepted sequential integer IDs and returned full user
records without requiring any authentication token or session. By incrementing the ID
value (1, 2, 123 ...), an attacker can enumerate all users in the system and retrieve
their data.

### Beginner-Friendly Explanation

Imagine a filing cabinet where every drawer is numbered. Anyone walking by can open
any drawer and read the files inside — no key needed, no identity check. This API
worked the same way. If you knew (or guessed) a user's ID number, you could read
their account details with a simple browser request.

### Evidence Summary

```http
GET /api/user/1 HTTP/1.1
Host: <TARGET_IP>:5002
```

```json
HTTP/1.1 200 OK
{
  "id": 1,
  "username": "<REDACTED>",
  "email": "<REDACTED>",
  "role": "<REDACTED>"
}
```

```http
GET /api/user/123 HTTP/1.1
Host: <TARGET_IP>:5002
```

```json
HTTP/1.1 200 OK
{
  "id": 123,
  "username": "<REDACTED>",
  "email": "<REDACTED>",
  "role": "<REDACTED>"
}
```

### Observed Signals

- Status code: `200 OK` for every valid ID — no auth challenge
- Response body: full user record returned each time
- No `WWW-Authenticate` header in response
- No session cookie or token was required in the request

### Impact

An attacker could write a simple loop to request `/api/user/1` through `/api/user/N`
and harvest every user record in the database — usernames, emails, roles — with no
credentials required.

### Recommendation

Enforce authentication on every user data endpoint. Add ownership checks so
authenticated users can only retrieve their own records. See `mitigation.md`.

### Validation Notes

- Expected secure behavior: `401 Unauthorized` when no token is provided
- Retest result: pending after fix
- Remaining risk: none if auth + ownership check are enforced correctly

---

## Finding 2 — Verbose Error Leakage via Stack Trace (Debug Mode Active)

| Field      | Value                                        |
|------------|----------------------------------------------|
| Severity   | 🔴 High                                      |
| Endpoint   | `GET /api/user/xyz`                          |
| Type       | Stack trace disclosure, debug mode in production |
| OWASP 2025 | A02 Security Misconfiguration + A10          |

### Description

Passing a non-integer string (`xyz`) to an endpoint that expects an integer caused
Python to raise a `ValueError`. With Werkzeug debug mode active, the full interactive
debugger was returned in the HTTP response — including the stack trace, internal file
paths, source code lines, and a flag embedded in the debug output.

### Beginner-Friendly Explanation

When a developer is building an app, they turn on "debug mode" so they can see
detailed error messages when things go wrong. This is useful during development.
The problem is that debug mode was never turned off before the app was deployed.
So when we sent unexpected input (`xyz` instead of a number), the app crashed and
showed us everything — the same detailed crash report a developer would see. That
report included internal file paths, source code, and sensitive data.

### Evidence Summary

```http
GET /api/user/xyz HTTP/1.1
Host: <TARGET_IP>:5002
```

```
HTTP/1.1 500 Internal Server Error
Content-Type: text/html

Werkzeug Debugger — ValueError

ValueError: invalid literal for int() with base 10: 'xyz'

Traceback:
  File "<REDACTED_SERVER_PATH>/app.py", line <REDACTED>, in get_user
    user_id = int(user_id)

<REDACTED — flag value was visible in debug output>
```

### Observed Signals

- Status code: `500 Internal Server Error`
- Response body: full Werkzeug HTML debug page
- Stack trace included: file paths, line numbers, source code context
- Exception type visible: `ValueError`
- Flag present in debug output (redacted here)
- Werkzeug interactive console PIN prompt visible in the page

### Impact

- Confirms `DEBUG=True` in production — a critical misconfiguration
- Internal file paths and source code structure exposed to any user
- Flag (sensitive data) leaked in debug output
- Werkzeug debug console, if PIN is obtained or brute-forced, enables
  **Remote Code Execution** directly from the browser

### Recommendation

Disable debug mode (`FLASK_DEBUG=0`). Add a global error handler. Use a production
WSGI server (Gunicorn). Add typed URL converters (`<int:user_id>`) to reject
non-integer input before it reaches the route. See `mitigation.md`.

### Validation Notes

- Expected secure behavior: `400 Bad Request` with generic `{"error": "Invalid user ID."}`
- Retest result: pending after fix
- Remaining risk: none if debug mode disabled and error handler added
