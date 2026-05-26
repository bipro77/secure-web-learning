# Remediation: A02 Security Misconfiguration

## Root Cause Summary

The application was deployed with developer settings active in a production environment.
No authentication was enforced on API endpoints, no input validation was applied to
user-controlled parameters, and the framework's debug mode was left enabled — causing
full stack traces to be returned to unauthenticated users on error.

## Findings and Fixes

| Finding                          | Priority  | Fix                                                  |
|----------------------------------|-----------|------------------------------------------------------|
| Debug mode active in production  | 🔴 High   | Set `FLASK_DEBUG=0`, use Gunicorn not `flask run`    |
| No auth on `/api/user/<id>`      | 🔴 High   | Add `@require_auth` decorator + ownership check      |
| No input validation on user ID   | 🔴 High   | Use `<int:user_id>` + positive range check           |
| Stack trace on type error        | 🔴 High   | Add global `@app.errorhandler(Exception)` handler    |
| Server header discloses stack    | 🟡 Medium | Suppress in `after_request`, use Nginx in production |
| Verbose error messages           | 🟡 Medium | Return generic messages, log details internally      |

## Recommended Fixes

### 1. Disable debug mode and use a production WSGI server

```bash
# Environment
FLASK_ENV=production
FLASK_DEBUG=0

# Run with Gunicorn, not flask run
gunicorn --bind 0.0.0.0:5002 --workers 4 app:app
```

### 2. Add a global error handler

```python
@app.errorhandler(Exception)
def handle_exception(e):
    app.logger.error(f"Unhandled: {e}", exc_info=True)
    return {"error": "An internal error occurred."}, 500
```

### 3. Validate user ID input

```python
@app.route('/api/user/<int:user_id>')
def get_user(user_id):
    if user_id <= 0:
        return {"error": "Invalid user ID."}, 400
    user = db.session.get(User, user_id)
    if not user:
        return {"error": "User not found."}, 404
    return jsonify(user.to_dict()), 200
```

### 4. Enforce authentication and ownership

```python
@app.route('/api/user/<int:user_id>')
@require_auth
def get_user(user_id):
    if current_user.id != user_id and not current_user.is_admin:
        return {"error": "Forbidden."}, 403
```

### 5. Suppress the Server header

```python
@app.after_request
def remove_server_header(response):
    response.headers['Server'] = 'webserver'
    return response
```

## Verification Steps

1. Confirm `DEBUG=False` — send `/api/user/xyz` and verify no stack trace is returned
2. Confirm auth is enforced — send `/api/user/1` with no token and verify 401 response
3. Confirm input validation — send `/api/user/-1` and verify 400 response
4. Confirm Server header is suppressed — run `curl -I` and verify no framework version shown
5. Confirm generic errors — no file paths, line numbers, or exception types in any response

## Full Mitigation Detail

See `tryhackme/user-management-api-misconfiguration/mitigation.md` for code examples
covering all five fixes with Flask/Python implementations.

