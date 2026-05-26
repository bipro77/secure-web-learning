# Methodology: API Enumeration and Verbose Error Leakage

## Scope

| Field              | Value                                           |
|--------------------|-------------------------------------------------|
| Platform           | TryHackMe                                       |
| Target             | `<TARGET_IP>:5002`                              |
| Date               | 2025-05-25                                      |
| Authorization      | TryHackMe lab — authorized testing only         |
| Tool               | Chrome browser (Pretty-print JSON), nmap        |
| OWASP 2025         | A02 — Security Misconfiguration                 |

## Objective

Identify API endpoints that enumerate user data without authentication, and trigger
verbose error responses that disclose internal framework details, stack traces, and
debug information through misconfigured deployment settings.

## Testing Principles

- Stay inside the assigned TryHackMe target only
- Change one variable at a time — one test per request
- Sanitize all evidence before committing (no flags, IPs, real usernames)
- Document what the app reveals, not how to exploit it further

---

## Phase 1 — Reconnaissance

### 1.1 Service Fingerprinting

```bash
nmap -sC -sV <TARGET_IP>
```

**Goal:** Identify open ports, services, and version banners.

**What was found:**

| Port | Service | Version                       |
|------|---------|-------------------------------|
| 22   | SSH     | OpenSSH                       |
| 5002 | HTTP    | Werkzeug/3.1.3 Python/3.11.14 |
| 5003 | HTTP    | Werkzeug/3.1.3 Python/3.11.14 |
| 5004 | HTTP    | Werkzeug/3.1.3 Python/3.11.14 |

**Key signal:** `Werkzeug` in the `Server` header means the app is running Flask's
built-in development server — not a production WSGI server. This strongly suggests
debug mode may be active.

---

## Phase 2 — API Enumeration

### 2.1 Baseline — Valid User IDs

Open Chrome, enable Pretty-print, and browse sequentially:

```
http://<TARGET_IP>:5002/api/user/1
http://<TARGET_IP>:5002/api/user/2
http://<TARGET_IP>:5002/api/user/123
```

**Goal:** Confirm the endpoint exists and returns user data without authentication.

**What to observe:**
- Does it return data without a login or token? → no auth enforced
- What fields are returned? → scope of data exposure
- Are IDs sequential and predictable? → full enumeration is trivial

### 2.2 Out-of-Range ID

```
http://<TARGET_IP>:5002/api/user/999999
```

**Goal:** Observe how the app handles a valid-type but non-existent ID.

**What to observe:**
- Does it return a clean `404`? → well handled
- Does the error message reveal internal detail? → information leakage
- Is there a difference between the "not found" and "found" response? → enumeration signal

---

## Phase 3 — Boundary and Type Confusion Testing

### 3.1 Negative ID

```
http://<TARGET_IP>:5002/api/user/-1
```

**Goal:** Test whether the app validates that IDs must be positive integers.

**What to observe:**
- Does it return `400 Bad Request`? → input validation present
- Does it crash with a 500? → no validation, no error handler

### 3.2 String Input (Type Confusion)

```
http://<TARGET_IP>:5002/api/user/xyz
```

**Goal:** Pass a string where an integer is expected to trigger an unhandled exception.

**What to observe:**
- Does it return a clean `400`? → typed URL converter present
- Does it return a `500` with a Werkzeug debug page? → `DEBUG=True` confirmed
- What does the stack trace reveal? (file paths, source code, embedded data)

> This is the highest-value test in this lab. A full Werkzeug stack trace confirms
> debug mode is active and exposes internal implementation details.

---

## Phase 4 — Evidence Capture

- Screenshot every response in Chrome with Pretty-print enabled
- Note: endpoint, HTTP method, input, status code, key response detail
- Redact all flag values, usernames, emails, and internal paths before saving
- File screenshots into `screenshots/` within this lab folder
- Record findings in `findings.md`
