# Findings: API Enumeration and Verbose Error Leakage

## Finding Summary

- Title:
- Severity:
- OWASP category: A04 Insecure Design
- Related category:
- Endpoint:
- Method:
- Affected parameter or identifier:
- Status:

## Finding Type

Select the relevant type:

- API enumeration
- Arbitrary ID access
- Verbose error leakage
- Stack trace disclosure
- Debug information exposure
- Improper exception handling

## Description

Describe the observed behavior. Focus on what the application revealed or allowed, how the behavior differed from the expected secure design, and why the result matters.

## Beginner-Friendly Explanation

Explain the issue in plain language. For example: changing an identifier, sending malformed input, or calling an unexpected endpoint produced a response that revealed more information than a normal user should receive.

## Evidence Summary

Use sanitized examples only.

```http
REQUEST_PLACEHOLDER
```

```http
RESPONSE_PLACEHOLDER
```

## Observed Signals

- Status code:
- Response body difference:
- Header difference:
- Error message detail:
- Debug or stack trace detail:
- Object or endpoint existence signal:

## Impact

Explain how the disclosed information could help an attacker understand the API, infer valid objects, identify framework behavior, or refine further testing. Do not include active exploit chains.

## Recommendation

Summarize the required fix and reference `mitigation.md`.

## Validation Notes

- Expected secure behavior:
- Retest result:
- Remaining risk:
