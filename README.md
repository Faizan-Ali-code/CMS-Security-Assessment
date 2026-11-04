# CCMS Application Security Assessment

Security assessment of a Customer Content Management System built on Astro and Strapi.

**[📄 Read the full report (PDF)](CCMS_Security_Assessment_Report.pdf)**

## Overview

A self-hosted lab deployment of CCMS was assessed using manual testing with Burp Suite, source code review and dependency scanning. The 34 findings are each described with their risk, business impact and remediation, and mapped to the OWASP Top 10 (2021).

## Results

| Severity | Findings | Still open |
| --- | :---: | :---: |
| Critical | 1 | 0 |
| High | 12 | 8 |
| Medium | 12 | 4 |
| Low | 9 | 3 |
| **Total** | **34** | **15** |

## Key findings

- **Authentication gaps:** no MFA or bot protection, a weak password policy and a race condition in the lockout counter
- **Workflow bypass:** an editor could publish content without reviewer approval
- **Browser defenses:** CSRF tokens not enforced; the CSP still allows inline scripts
- **Fixed:** a JWT that stayed valid after logout (Critical), broken access control and an open CORS policy

## Tools and skills

Burp Suite · Source code review · Dependency scanning · OWASP Top 10 · Risk rating · Security reporting

## Disclaimer

Performed on a self-hosted lab deployment for learning and portfolio purposes. The report contains no exploit code or raw evidence.

## Author

**Faizan Ali**: [GitHub](https://github.com/Faizan-Ali-code)
