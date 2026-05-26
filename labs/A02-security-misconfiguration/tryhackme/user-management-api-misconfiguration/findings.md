# Findings — User Management API Misconfiguration

## Summary

| # | Finding                                    | Severity  | Endpoint              |
|---|--------------------------------------------|-----------|-----------------------|
| 1 | Server header discloses framework/version  | 🟡 Medium | All responses         |
| 2 | Unauthenticated access to user data        | 🔴 High   | `GET /api/user/123`   |
| 3 | No auth check on arbitrary out-of-range ID | 🔴 High   | `GET /api/user/999999`|
| 4 | No input validation — negative ID accepted | 🔴 High   | `GET /api/user/-1`    |
| 5 | Type confusion triggers full stack trace   | 🔴 High   | `GET /api/user/xyz`   |

---

## Finding 1 — Server Header Discloses Framework and Runtime Version

| Field      | Value                                        |
|------------|----------------------------------------------|
| Severity   | 🟡 Medium                                    |
| Source     | nmap -sC -sV / HTTP response headers         |
| OWASP 2025 | A02 — Security Misconfiguration              |

### Evidence

```
nmap -sC -sV <TARGET_IP>

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH
5002/tcp open  http    Werkzeug/3.1.3 Python/3.11.14
5003/tcp open  http    Werkzeug/3.1.3 Python/3.11.14
5004/tcp open  http    Werkzeug/3.1.3 Python/3.11.14
```

```
HTTP Response Header:
Server: Werkzeug/3.1.3 Python/3.11.14
```

### Screenshot
`screenshots/01-recon-nmap-service-scan.png`

### Impact
- Confirms the exact framework (Werkzeug = Flask dev server — not production-grade)
- Reveals the Python runtime version
- Allows an attacker to search for known CVEs specific to Werkzeug 3.1.3
- Confirms debug mode is likely active (Werkzeug dev server ships with interactive debugger)
- Three exposed ports (5002, 5003, 5004) indicate multiple microservices — expanded attack surface

### Root Cause
The Werkzeug development server was used in production instead of a proper WSGI server
(Gunicorn, uWSGI). The `Server` response header was never suppressed or replaced.

---

## Finding 2 — Unauthenticated Access to User Data (IDOR)

| Field      | Value                                        |
|------------|----------------------------------------------|
| Severity   | 🔴 High                                      |
| Endpoint   | `GET /api/user/123`                          |
| Method     | GET                                          |
| OWASP 2025 | A02 — Security Misconfiguration (+ A01 IDOR) |

### Evidence

```http
GET /api/user/123 HTTP/1.1
Host: <TARGET_IP>:5002
```

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "username": "<REDACTED>",
  "email": "<REDACTED>",
  "role": "<REDACTED>"
}
```

No `Authorization` header was sent. The server returned full user data with no authentication.

### Screenshot
`screenshots/02-enum-api-user-123-idor.png`

### Impact
- Any unauthenticated user can retrieve any user record by guessing an integer ID
- User PII (username, email, role) exposed without any credential check
- Sequential IDs make enumeration trivial — an attacker could scrape all user records

### Root Cause
Authentication was never enforced at the route level. The endpoint had no `@require_auth`
decorator or token validation — a deployment gap, not a code logic error.

---

## Finding 3 — No Authorization Check on Arbitrary Out-of-Range ID

| Field      | Value                                        |
|------------|----------------------------------------------|
| Severity   | 🔴 High                                      |
| Endpoint   | `GET /api/user/999999`                       |
| Method     | GET                                          |
| OWASP 2025 | A02 — Security Misconfiguration              |

### Evidence

```http
GET /api/user/999999 HTTP/1.1
Host: <TARGET_IP>:5002
```

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "error": "User <REDACTED> not found",
  "debug_info": "<REDACTED>"
}
```

The response confirmed the request was processed (not rejected at the boundary),
and the error message revealed internal details about how the lookup was performed.

### Screenshot
`screenshots/03-enum-api-user-999999-idor-no-auth.png`

### Impact
- Confirms that no auth check occurs before processing the request
- Error message wording reveals internal implementation detail (object existence signal)
- Response difference between valid and invalid IDs enables user enumeration

### Root Cause
No authentication enforced. No input range validation before the database query.
Error messages not sanitized — raw lookup detail returned to client.

---

## Finding 4 — No Input Validation — Negative ID Accepted and Processed

| Field      | Value                                        |
|------------|----------------------------------------------|
| Severity   | 🔴 High                                      |
| Endpoint   | `GET /api/user/-1`                           |
| Method     | GET                                          |
| OWASP 2025 | A02 — Security Misconfiguration              |

### Evidence

```http
GET /api/user/-1 HTTP/1.1
Host: <TARGET_IP>:5002
```

```json
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "error": "<REDACTED internal error detail>",
  "traceback": "<REDACTED>"
}
```

The server accepted `-1` as a valid input and attempted to process it,
resulting in a 500 error with verbose error detail rather than a clean 400 rejection.

### Screenshot
`screenshots/04-exploit-api-negative-id-verbose-error.png`

### Impact
- Confirms absence of any positive-integer range validation
- 500 error + verbose output reveals that the application does not gracefully handle
  boundary input — further confirming debug mode is active
- Provides additional internal detail to an attacker

### Root Cause
No range check (`if user_id <= 0: return 400`) before processing the request.
No global error handler to catch and suppress the resulting 500 exception output.

---

## Finding 5 — Type Confusion Triggers Full Werkzeug Stack Trace (Flag Disclosed)

| Field      | Value                                        |
|------------|----------------------------------------------|
| Severity   | 🔴 High                                      |
| Endpoint   | `GET /api/user/xyz`                          |
| Method     | GET                                          |
| OWASP 2025 | A02 — Security Misconfiguration + A10        |

### Evidence

```http
GET /api/user/xyz HTTP/1.1
Host: <TARGET_IP>:5002
```

```
HTTP/1.1 500 Internal Server Error
Content-Type: text/html

Werkzeug Interactive Debugger
ValueError: invalid literal for int() with base 10: 'xyz'

Traceback (most recent call last):
  File "<REDACTED_PATH>/app.py", line <REDACTED>, in get_user
    user_id = int(user_id)

<REDACTED flag value present in debug output>
```

Passing a string where the application expected an integer caused Python to raise a
`ValueError`. With `DEBUG=True`, Werkzeug returned the full interactive debug page
including the stack trace, source code context, internal file paths, and a flag
embedded in the debug output.

### Screenshot
`screenshots/05-exploit-api-xyz-stack-trace-disclosure.png`

### Impact
- **Highest severity finding in this lab**
- Confirms `DEBUG=True` is active in production
- Exposes internal file paths and application source code structure
- Flag was visible in the debug output — demonstrating real data disclosure
- Werkzeug debug mode also activates an interactive Python console
  accessible via a PIN — if leaked, this enables **Remote Code Execution**

### Root Cause
- `DEBUG=True` left enabled from development — not changed for production deployment
- No Flask typed URL converter (`<int:user_id>`) to reject non-integer input before it reached the route
- No global `@app.errorhandler(Exception)` to catch and suppress unhandled exceptions

