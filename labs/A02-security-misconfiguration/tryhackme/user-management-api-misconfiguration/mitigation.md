# Mitigation — User Management API Misconfiguration

## Overview

All findings in this lab are misconfigurations — not code bugs. Every fix below is a
deployment, configuration, or defensive coding decision, not a rewrite of business logic.

---

## Fix 1 — Disable Debug Mode in Production

**Problem:**
Werkzeug's debug mode was active on a production-facing service. When an unhandled
exception occurred (e.g., passing `xyz` as a user ID), the full interactive debugger
and stack trace were returned in the HTTP response, exposing file paths, source code
lines, and Python runtime internals.

**Root cause:** `DEBUG=True` was left on from development. The Werkzeug development
server is not designed for production and should never be internet-facing.

**Fix — environment variable:**
```bash
# .env or deployment config
FLASK_ENV=production
FLASK_DEBUG=0
```

**Fix — application code:**
```python
import os
from flask import Flask

app = Flask(__name__)

# Read from environment — never hardcode True
app.config['DEBUG'] = os.environ.get('FLASK_DEBUG', '0') == '1'
```

**Fix — global error handler (always add this regardless of debug setting):**
```python
from flask import jsonify

@app.errorhandler(Exception)
def handle_exception(e):
    # Log internally for ops visibility
    app.logger.error(f"Unhandled exception: {e}", exc_info=True)
    # Return nothing useful to the client
    return jsonify({"error": "An internal error occurred."}), 500

@app.errorhandler(404)
def not_found(e):
    return jsonify({"error": "Resource not found."}), 404

@app.errorhandler(400)
def bad_request(e):
    return jsonify({"error": "Bad request."}), 400
```

---

## Fix 2 — Validate and Sanitize User ID Input

**Problem:**
The `/api/user/<id>` endpoint accepted any value as a user ID without validation.
Passing a string (`xyz`) or negative integer (`-1`) caused unhandled exceptions or
unexpected database behaviour instead of a controlled rejection.

**Fix — use Flask's typed URL converter:**
```python
# Flask will automatically return 404 if <id> is not an integer
@app.route('/api/user/<int:user_id>')
def get_user(user_id):
    ...
```

**Fix — additional range validation:**
```python
@app.route('/api/user/<int:user_id>')
def get_user(user_id):
    if user_id <= 0:
        return jsonify({"error": "Invalid user ID."}), 400

    user = db.session.get(User, user_id)
    if not user:
        return jsonify({"error": "User not found."}), 404

    return jsonify(user.to_dict()), 200
```

**Why this matters:**
- Prevents type confusion errors that trigger stack traces
- Stops boundary probing from leaking database behaviour
- Consistent 400/404 responses give attackers no useful information

---

## Fix 3 — Add Authentication to API Endpoints

**Problem:**
Any unauthenticated request to `/api/user/<id>` returned full user data. There was no
session check, token validation, or identity verification of any kind. This is a
combined misconfiguration (no auth enforced) and insecure design issue.

**Fix — require a valid token on all user data routes:**
```python
from functools import wraps
from flask import request, jsonify

def require_auth(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        token = request.headers.get('Authorization', '').replace('Bearer ', '')
        if not token or not is_valid_token(token):
            return jsonify({"error": "Unauthorized."}), 401
        return f(*args, **kwargs)
    return decorated

@app.route('/api/user/<int:user_id>')
@require_auth
def get_user(user_id):
    # Also enforce: authenticated user can only access their own record
    if current_user.id != user_id and not current_user.is_admin:
        return jsonify({"error": "Forbidden."}), 403
    ...
```

---

## Fix 4 — Suppress the Server Response Header

**Problem:**
The `Server: Werkzeug/3.1.3 Python/3.11.14` header was returned on every response,
advertising the exact framework and runtime version to anyone who looked.

**Fix — remove or replace the Server header in Flask:**
```python
from flask import Flask

app = Flask(__name__)

@app.after_request
def remove_server_header(response):
    response.headers['Server'] = 'webserver'   # generic replacement
    # Or remove entirely:
    # response.headers.remove('Server')
    return response
```

**Fix — use a production WSGI server (not Werkzeug's built-in):**

Werkzeug's built-in server is for development only. In production, run Flask behind
a proper WSGI server such as **Gunicorn** or **uWSGI**, then behind a reverse proxy
such as **Nginx** or **Caddy** which controls headers.

```bash
# Run with Gunicorn instead of flask run
gunicorn --bind 0.0.0.0:5002 --workers 4 app:app
```

```nginx
# Nginx — suppress upstream Server header
proxy_hide_header Server;
add_header Server "webserver" always;
```

---

## Fix 5 — Use Generic Error Messages in Production

**Problem:**
Error responses from the API revealed internal details: which user ID was queried,
what type was received, and what the application expected. Even non-stack-trace errors
can assist an attacker in mapping behaviour.

**Rule:** Production error responses should answer *that* something went wrong,
never *why* in technical detail.

| Situation              | Bad response (leaks info)                          | Good response          |
|------------------------|----------------------------------------------------|------------------------|
| User ID not found      | `"No user with id=999999 in database"`             | `"User not found."`    |
| Invalid type input     | `"ValueError: invalid literal for int(): 'xyz'"`   | `"Invalid user ID."`   |
| Negative ID            | `"UserID must be positive, got -1"`                | `"Invalid user ID."`   |
| Unhandled exception    | Full Werkzeug stack trace with file paths          | `"Internal error."`    |

```python
# Consistent, information-free error responses
ERRORS = {
    "invalid_id":  ({"error": "Invalid user ID."}, 400),
    "not_found":   ({"error": "User not found."}, 404),
    "unauthorized":({"error": "Unauthorized."}, 401),
    "forbidden":   ({"error": "Forbidden."}, 403),
    "internal":    ({"error": "An internal error occurred."}, 500),
}
```

---

## Fix Summary

| Finding                         | Fix                                              | Priority |
|---------------------------------|--------------------------------------------------|----------|
| Debug mode active in production | Set `FLASK_DEBUG=0`, add global error handlers   | 🔴 High  |
| No input validation on user ID  | Use `<int:user_id>`, add range checks            | 🔴 High  |
| No authentication on endpoints  | Add token auth decorator + ownership check       | 🔴 High  |
| Server header discloses stack   | Suppress header, use Gunicorn + Nginx            | 🟡 Medium|
| Verbose error messages          | Return generic messages, log internally          | 🟡 Medium|
