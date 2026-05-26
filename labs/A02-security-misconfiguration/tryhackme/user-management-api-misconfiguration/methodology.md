# Methodology — User Management API Misconfiguration

## Target

| Field         | Value                                      |
|---------------|--------------------------------------------|
| Platform      | TryHackMe                                  |
| Port          | 5002 (primary target)                      |
| Tool          | Chrome browser (Pretty-print JSON), nmap   |
| Authorization | TryHackMe lab — authorized testing only    |

---

## Phase 1 — Reconnaissance

### 1.1 Port Scan

Run a service/version scan against the target machine to identify open ports and running services.

```bash
nmap -sC -sV <MACHINE_IP>
```

**Results:**

| Port | State | Service | Version                        |
|------|-------|---------|--------------------------------|
| 22   | open  | SSH     | OpenSSH                        |
| 5002 | open  | HTTP    | Werkzeug/3.1.3 Python/3.11.14  |
| 5003 | open  | HTTP    | Werkzeug/3.1.3 Python/3.11.14  |
| 5004 | open  | HTTP    | Werkzeug/3.1.3 Python/3.11.14  |

**Key observations from nmap:**
- Three separate HTTP services running on adjacent ports — likely different microservices or API modules
- All three HTTP services identify themselves as **Werkzeug 3.1.3** running on **Python 3.11.14**
- Framework and runtime version disclosed in the `Server` response header — a direct misconfiguration
- SSH on port 22 is out of scope for this challenge

### 1.2 Server Header Disclosure

The `Server` header returned by the application exposes the full technology stack:

```
Server: Werkzeug/3.1.3 Python/3.11.14
```

**Why this matters:**
- Tells an attacker the exact framework (Werkzeug = Flask development server)
- Reveals the Python version
- Werkzeug's built-in development server is not production-grade and ships with a debug console
- Known CVEs for specific Werkzeug versions can be targeted directly

---

## Phase 2 — API Endpoint Discovery

### 2.1 Navigate to the target service

Open Chrome and go to:

```
http://<MACHINE_IP>:5002/
```

Use Chrome's **Pretty-print** toggle on JSON responses for readable output.

### 2.2 User enumeration by sequential ID

Test the `/api/user/<id>` endpoint with valid, boundary, and invalid IDs:

| Request                          | Intent                             |
|----------------------------------|------------------------------------|
| `GET /api/user/1`                | Valid low ID — does a user exist?  |
| `GET /api/user/123`              | Valid higher ID — enumerate users  |
| `GET /api/user/999999`           | Out-of-range ID — how does it fail?|
| `GET /api/user/-1`               | Boundary test — negative value     |
| `GET /api/user/xyz`              | Type confusion — string not integer|

```
http://<MACHINE_IP>:5002/api/user/1
http://<MACHINE_IP>:5002/api/user/123
http://<MACHINE_IP>:5002/api/user/999999
http://<MACHINE_IP>:5002/api/user/-1
http://<MACHINE_IP>:5002/api/user/xyz
```

---

## Phase 3 — Boundary and Type Confusion Testing

### 3.1 Out-of-range integer — `/api/user/999999`

**What to look for:** Does the app return a clean 404, or does it leak internal details?

- A well-configured app returns: `{"error": "User not found"}` with HTTP 404
- A misconfigured app may return a stack trace, database error, or internal path

### 3.2 Negative value boundary — `/api/user/-1`

**What to look for:** Negative IDs are not valid user IDs. The app should reject this.

- A well-configured app returns: `{"error": "Invalid user ID"}` with HTTP 400
- A misconfigured app may attempt a database lookup with `-1` and return a verbose error or unexpected data

### 3.3 Type confusion — `/api/user/xyz`

**What to look for:** A string where an integer is expected should be caught at input validation.

- A well-configured app returns: `{"error": "Invalid user ID"}` with HTTP 400
- A misconfigured app in debug mode may return a **full Werkzeug stack trace** including:
  - File paths on the server
  - Line numbers and source code snippets
  - The exact exception type and message
  - Python runtime internals

> This is the highest-severity finding — a stack trace from a type error reveals the
> server's internal structure and confirms debug mode is active in production.

---

## Phase 4 — Evidence Capture

- Screenshot each response in Chrome with Pretty-print enabled
- Name screenshots sequentially: `01-`, `02-`, `03-` etc.
- Record endpoint, input, HTTP status code, and key response content in `findings.md`
- Never save actual flag values — note that a flag was present and redact the value
