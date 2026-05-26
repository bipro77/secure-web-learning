# Mitigation — User Management API Misconfiguration

## Fix 1 — Suppress Verbose Error Messages

**Problem:** Stack traces, file paths, or system internals returned in error responses.

**Fix:**
- Disable debug mode in production (`DEBUG=False`)
- Implement a global error handler that returns generic messages only

```python
# Flask example
@app.errorhandler(Exception)
def handle_exception(e):
    return {"error": "An internal error occurred."}, 500
```

```javascript
// Express example
app.use((err, req, res, next) => {
  res.status(500).json({ error: 'Internal server error' });
});
```

---

## Fix 2 — Restrict API Endpoint Access

**Problem:** API endpoints return data without authentication or authorization checks.

**Fix:**
- Require authentication tokens on all user data endpoints
- Validate that the requesting user can only access their own data (IDOR prevention)

```python
@app.route('/api/users/<int:user_id>')
@require_auth
def get_user(user_id):
    if current_user.id != user_id and not current_user.is_admin:
        return {"error": "Forbidden"}, 403
```

---

## Fix 3 — Remove Debug Traces and Exposed Routes

**Problem:** Development or debug endpoints left enabled in production.

**Fix:**
- Audit and remove `/debug`, `/env`, `/config`, `/status` routes before deployment
- Use environment-based feature flags to disable dev-only routes

```python
if app.config['DEBUG']:
    @app.route('/debug')
    def debug_info():
        ...
```

---

## Fix 4 — Validate and Sanitize Input

**Problem:** Unexpected input (strings, negatives, nulls) triggers verbose errors.

**Fix:**
- Validate all input at the API boundary before processing
- Return controlled 400 Bad Request responses for invalid input

```python
@app.route('/api/users/<user_id>')
def get_user(user_id):
    if not user_id.isdigit():
        return {"error": "Invalid user ID"}, 400
```

---

## Fix 5 — Remove Sensitive Response Headers

**Problem:** `Server` or `X-Powered-By` headers reveal stack details.

**Fix:**
```python
# Flask — remove Server header
app.config['SERVER_NAME'] = None

# Express
app.disable('x-powered-by')
```
