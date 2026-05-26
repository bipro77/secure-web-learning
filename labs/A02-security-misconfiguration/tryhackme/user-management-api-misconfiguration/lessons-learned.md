# Lessons Learned — User Management API Misconfiguration

## Key Takeaways

<!-- Fill in after completing the lab -->

- [ ] What misconfigurations were present?
- [ ] What information was disclosed?
- [ ] What could an attacker do with this?
- [ ] What was the simplest fix?

## Concepts Reinforced

- Security misconfiguration does not require a code bug — deployment and config mistakes are enough
- Verbose errors are a dual risk: they help attackers enumerate and understand the system
- APIs need the same hardening as UI-facing endpoints
- Development artifacts (debug routes, stack traces) must be stripped before production

## Follow-Up Study

- OWASP Security Misconfiguration: https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/
- OWASP API Security Top 10: https://owasp.org/API-Security/
- Flask security best practices: https://flask.palletsprojects.com/en/stable/security/
- Express security best practices: https://expressjs.com/en/advanced/best-practice-security.html
