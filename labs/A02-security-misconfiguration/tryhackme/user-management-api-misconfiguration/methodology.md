# Methodology — User Management API Misconfiguration

## Target

- URL: http://MACHINE_IP:5002
- Scope: User Management API endpoints only
- Authorization: TryHackMe lab — authorized testing

---

## Phase 1 — Reconnaissance (Browser + curl)

### 1.1 Visit the base URL

```
http://MACHINE_IP:5002/
```

Look for:
- Any visible links, forms, or API references
- Response headers (Server, X-Powered-By, Content-Type)
- HTML source — comments, hidden fields, hardcoded paths

### 1.2 Check response headers

```bash
curl -I http://MACHINE_IP:5002/
```

Look for:
- `Server:` header (reveals framework/version)
- `X-Powered-By:` (e.g., Express, PHP)
- Missing security headers (no `X-Content-Type-Options`, no `X-Frame-Options`)

---

## Phase 2 — API Endpoint Discovery

### 2.1 Try common API paths

```bash
curl http://MACHINE_IP:5002/api
curl http://MACHINE_IP:5002/api/users
curl http://MACHINE_IP:5002/api/user
curl http://MACHINE_IP:5002/users
curl http://MACHINE_IP:5002/admin
curl http://MACHINE_IP:5002/debug
curl http://MACHINE_IP:5002/health
curl http://MACHINE_IP:5002/status
curl http://MACHINE_IP:5002/info
curl http://MACHINE_IP:5002/env
curl http://MACHINE_IP:5002/config
```

### 2.2 Check for API documentation exposure

```bash
curl http://MACHINE_IP:5002/docs
curl http://MACHINE_IP:5002/swagger
curl http://MACHINE_IP:5002/swagger.json
curl http://MACHINE_IP:5002/openapi.json
curl http://MACHINE_IP:5002/api-docs
curl http://MACHINE_IP:5002/redoc
```

### 2.3 Try user enumeration by ID

```bash
curl http://MACHINE_IP:5002/api/users/1
curl http://MACHINE_IP:5002/api/users/2
curl http://MACHINE_IP:5002/api/users/0
curl http://MACHINE_IP:5002/api/users/-1
curl http://MACHINE_IP:5002/api/users/abc
curl http://MACHINE_IP:5002/api/users/999999
curl http://MACHINE_IP:5002/api/users/null
```

---

## Phase 3 — Trigger Verbose Errors

### 3.1 Send unexpected input types

```bash
# String where integer expected
curl http://MACHINE_IP:5002/api/users/abc

# Special characters
curl http://MACHINE_IP:5002/api/users/'

# Negative values
curl http://MACHINE_IP:5002/api/users/-1

# Very large number
curl http://MACHINE_IP:5002/api/users/9999999999
```

Look for:
- Stack traces (file paths, line numbers, framework internals)
- Database error messages (SQL, table names)
- Internal hostnames or IP addresses
- Debug information or flag disclosure

### 3.2 Try different HTTP methods

```bash
curl -X POST   http://MACHINE_IP:5002/api/users
curl -X PUT    http://MACHINE_IP:5002/api/users/1
curl -X DELETE http://MACHINE_IP:5002/api/users/1
curl -X PATCH  http://MACHINE_IP:5002/api/users/1
curl -X OPTIONS http://MACHINE_IP:5002/api/users
```

### 3.3 Send malformed Content-Type or body

```bash
curl -X POST http://MACHINE_IP:5002/api/users \
  -H "Content-Type: application/json" \
  -d '{"invalid": true}'

curl -X POST http://MACHINE_IP:5002/api/users \
  -H "Content-Type: application/xml" \
  -d '<user><id>1</id></user>'
```

---

## Phase 4 — Document Findings

- Screenshot every response that reveals internal information
- Note exact endpoint, method, payload, and response
- Record in findings.md
- Capture screenshots into screenshots/
