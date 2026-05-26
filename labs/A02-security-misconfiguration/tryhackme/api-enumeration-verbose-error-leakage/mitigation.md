# Mitigation: API Enumeration and Verbose Error Leakage

## Root Cause

Both findings in this lab stem from the same root cause: the application was deployed
with development settings active. API enumeration succeeded because authentication was
never applied to the route. Verbose error leakage occurred because `DEBUG=True` was
left enabled, causing Werkzeug to return full stack traces instead of generic errors.
Neither issue requires a code rewrite — both are fixed by deployment configuration
and defensive coding patterns.

---

## Fix 1 — Stop Verbose Error Leakage: Disable Debug Mode

**Problem:** `DEBUG=True` caused full Werkzeug stack traces to be returned on any
unhandled exception, exposing file paths, source code, and sensitive data.

```bash
# Set in environment — never hardcode
FLASK_DEBUG=0
FLASK_ENV=production
```

```python
# app.py — read from environment, default to off
import os
app.config['DEBUG'] = os.environ.get('FLASK_DEBUG', '0') == '1'
```

**Also replace Werkzeug dev server with Gunicorn:**

```bash
# Never use: flask run (for production)
# Use instead:
gunicorn --bind 0.0.0.0:5002 --workers 4 app:app
```

---

## Fix 2 — Add a Global Exception Handler

**Problem:** Unhandled exceptions (e.g., `ValueError` from `int('xyz')`) escaped to
the client as raw Werkzeug debug pages.

```python
from flask import jsonify
import logging

@app.errorhandler(Exception)
def handle_exception(e):
    # Log full detail server-side only
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

## Fix 3 — Validate Input Type and Range at the Route

**Problem:** `/api/user/xyz` and `/api/user/-1` were accepted without validation,
reaching application code and triggering unhandled exceptions.

```python
# Use Flask's typed URL converter — rejects non-integers automatically
@app.route('/api/user/<int:user_id>')
def get_user(user_id):
    # Also reject out-of-range values
    if user_id <= 0:
        return jsonify({"error": "Invalid user ID."}), 400

    user = db.session.get(User, user_id)
    if not user:
        return jsonify({"error": "User not found."}), 404

    return jsonify(user.to_dict()), 200
```

---

## Fix 4 — Enforce Authentication to Stop Enumeration

**Problem:** `/api/user/<id>` returned data with no authentication. Any sequential
integer ID returned the corresponding user's full record.

```python
from functools import wraps
from flask import request, jsonify, g

def require_auth(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        token = request.headers.get('Authorization', '').replace('Bearer ', '')
        user = verify_token(token)
        if not user:
            return jsonify({"error": "Unauthorized."}), 401
        g.current_user = user
        return f(*args, **kwargs)
    return decorated

@app.route('/api/user/<int:user_id>')
@require_auth
def get_user(user_id):
    # Ownership check — users can only access their own record
    if g.current_user.id != user_id and not g.current_user.is_admin:
        return jsonify({"error": "Forbidden."}), 403

    user = db.session.get(User, user_id)
    if not user:
        return jsonify({"error": "User not found."}), 404

    return jsonify(user.to_dict()), 200
```

---

## Secure Response Goals

| Scenario                    | Bad (current)                            | Good (fixed)                  |
|-----------------------------|------------------------------------------|-------------------------------|
| No auth token sent          | `200 OK` + full user data                | `401 Unauthorized`            |
| Valid ID, other user's data | `200 OK` + their full record             | `403 Forbidden`               |
| Out-of-range ID             | Verbose error with internal detail       | `404` + `"User not found."`   |
| Negative ID                 | `500` + stack trace                      | `400` + `"Invalid user ID."`  |
| String ID (`xyz`)           | Full Werkzeug debug page + flag leaked   | `400` + `"Invalid user ID."`  |

---

## Verification Steps

1. Send `GET /api/user/1` with no token → expect `401 Unauthorized`
2. Send `GET /api/user/1` with another user's valid token → expect `403 Forbidden`
3. Send `GET /api/user/-1` → expect `400 Bad Request`, generic message only
4. Send `GET /api/user/xyz` → expect `400 Bad Request`, no stack trace
5. Send `GET /api/user/999999` with valid auth → expect `404`, no internal detail
6. Run `curl -I http://<TARGET>:5002/` → `Server` header should not reveal Werkzeug/version
