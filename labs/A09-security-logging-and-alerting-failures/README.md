# A09:2025 — Security Logging and Alerting Failures

> Previously: A09:2021 — Security Logging and Monitoring Failures

## Description

Without sufficient logging and alerting, breaches cannot be detected, incident response
is delayed, and forensic investigation is impossible. This category covers missing audit
trails, silent failures, and monitoring blind spots.

## OWASP Reference

https://owasp.org/Top10/2025/A09_2025-Security_Logging_and_Alerting_Failures/

## Key Concepts

- Missing security-relevant event logs (logins, failures, privilege changes)
- Incomplete or tamper-able audit trails
- Poor or absent alerting on suspicious behavior
- Log injection attacks
- Logs stored locally (destroyable by attacker)
- Delayed or absent incident detection
- Sensitive data logged in plaintext

## Lab Platforms

- [ ] TryHackMe
- [ ] PortSwigger

## Tools

- Application and server logs
- Browser developer tools
- Burp Suite
- SIEM or logging dashboard (when available in lab)

## Workflow

1. Identify security-relevant events to test (login, failed auth, privilege change).
2. Trigger controlled events in authorized scope.
3. Check whether logs capture: who, what, when, where, and outcome.
4. Validate whether alerting would fire on suspicious sequences.
5. Capture evidence in `screenshots/`.
6. Record findings and remediation.

## Checklist

- [ ] Security-relevant events identified
- [ ] Logging behavior reviewed
- [ ] Log completeness verified (who/what/when/where/outcome)
- [ ] Alerting configuration reviewed
- [ ] Log injection tested
- [ ] Screenshots captured
- [ ] Remediation written

## Notes

See `notes.md` for findings and methodology.

