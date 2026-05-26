# Methodology: API Enumeration and Verbose Error Leakage

## Scope

- Platform: TryHackMe
- Room:
- Target: `<TARGET_HOST>`
- Date:
- Authorization notes:

## Objective

Identify API endpoints, arbitrary ID access patterns, response differences, verbose errors, stack traces, debug fields, and improper exception handling that disclose excessive technical detail in the lab environment.

## Testing Principles

- Stay inside the assigned TryHackMe target and room instructions.
- Change one variable at a time so observations are easy to explain.
- Prefer clear notes over raw tool dumps.
- Sanitize all requests, responses, screenshots, and identifiers before committing.
- Avoid documenting active exploit chains.

## Workflow

1. Confirm the lab target and allowed testing scope.
2. Browse the application normally and capture baseline API requests.
3. Review routes, methods, parameters, identifiers, status codes, and response shapes.
4. Compare valid and invalid resource identifiers such as `<RESOURCE_ID>`.
5. Test expected and unsupported HTTP methods.
6. Submit malformed, incomplete, or unexpected lab-safe requests to observe validation behavior.
7. Look for debug fields, stack traces, framework messages, database errors, file paths, and inconsistent responses.
8. Record only sanitized requests, responses, and screenshots.

## Methodology Sections

### 1. Baseline Mapping

- Identify visible API calls.
- Note endpoint names, HTTP methods, parameters, and normal status codes.
- Record expected application behavior.

### 2. API Enumeration Checks

- Compare known and unknown routes.
- Compare valid and invalid resource identifiers.
- Compare allowed and unsupported HTTP methods.
- Observe whether response differences reveal endpoint existence, object existence, or application state.

### 3. Arbitrary ID Access Checks

- Identify user-controlled identifiers.
- Replace identifiers with sanitized placeholders such as `<RESOURCE_ID>`.
- Compare access behavior across valid, invalid, and unauthorized object references.
- Document whether authorization appears to happen server-side.

### 4. Verbose Error and Stack Trace Checks

- Send malformed lab-safe input.
- Review error body, headers, and status code.
- Note any stack trace, exception type, framework version, database message, debug flag, or internal path.

### 5. Exception Handling Review

- Check whether errors are consistent across similar endpoints.
- Confirm whether client-facing messages are generic.
- Identify where detailed diagnostic information should be moved to protected server-side logs.

## Data Handling

- Replace secrets with `<REDACTED>`.
- Replace session material with `<SESSION_COOKIE>` or `<TOKEN>`.
- Replace target-specific values with `<TARGET_HOST>`, `<RESOURCE_ID>`, or `<USER_ID>`.
- Do not store real flags, credentials, target IPs, tokens, or session data.
