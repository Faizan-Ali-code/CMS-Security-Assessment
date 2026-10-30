<div align="center">

# CCMS Application Security Assessment

**Application security assessment of a Customer Content Management System built on Astro and Strapi —
34 findings with risk analysis, OWASP Top 10 mapping and a prioritized remediation roadmap.**

![findings](https://img.shields.io/badge/findings-34-1F4E78?style=flat-square) ![critical](https://img.shields.io/badge/critical-1-7B1D1D?style=flat-square) ![high](https://img.shields.io/badge/high-12-D92D20?style=flat-square) ![medium](https://img.shields.io/badge/medium-12-DC8A06?style=flat-square) ![low](https://img.shields.io/badge/low-9-4A8BC9?style=flat-square) ![OWASP](https://img.shields.io/badge/OWASP-Top%2010%20(2021)-333333?style=flat-square)

### [📄 Read the full report (PDF)](CCMS_Security_Assessment_Report.pdf) &nbsp;·&nbsp; [Editable version (DOCX)](CCMS_Security_Assessment_Report.docx)

</div>

---

## Overview

This repository contains a professional application security assessment report for **CCMS (Customer Content Management System)**, a content platform with a role-based editorial workflow, media uploads, document import and an AI chat assistant. The assessment was performed against a self-hosted lab deployment using manual testing, source code review and dependency analysis.

The report covers:

- An executive summary with severity and remediation-status breakdowns
- Scope, methodology and rating definitions (severity, priority, status)
- A consolidated register of all 34 findings, categorized and mapped to the OWASP Top 10 (2021)
- Detailed write-ups for each finding — summary, technical detail, security risk, business impact and remediation
- A prioritized remediation roadmap and a release recommendation

## Target Application

| | |
| --- | --- |
| **Application** | Customer Content Management System (CCMS) |
| **Architecture** | Astro server-rendered frontend (Node adapter, Preact components) backed by a Strapi 5 headless CMS API |
| **Stack** | Astro · @astrojs/node ^10.1.3 · @astrojs/preact ^5.1.5 · Strapi 5.47.1 |
| **Environment** | Self-hosted lab deployment, tested with unauthenticated and authenticated sessions across editorial roles |

## Scope & Methodology

| | |
| --- | --- |
| **Features in scope** | Authentication and sessions, role-based editorial workflow (editor/reviewer), media uploads, document import, snippets and variables, document tagging, AI chat assistant, HTTP/browser security controls and third-party dependencies |
| **Testing methods** | Manual web and API testing through an intercepting proxy; source code review of frontend and backend; dependency scanning (software composition analysis) |
| **Tools** | Burp Suite; dependency scanning tooling |
| **Out of scope** | Network and infrastructure penetration testing, social engineering, and volumetric denial-of-service or load testing |

**Approach:** Reconnaissance → Authentication & sessions → Authorization & workflow → Input & browser controls → Code & dependency review → Reporting

## Results at a Glance

| Severity | Open | Not Fixed | Partially Fixed | Closed | Total |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Critical** | 0 | 0 | 0 | 1 | **1** |
| **High** | 3 | 4 | 1 | 4 | **12** |
| Medium | 0 | 3 | 1 | 8 | **12** |
| Low | 1 | 2 | 0 | 6 | **9** |
| **Total** | **4** | **9** | **2** | **19** | **34** |

**Key risk themes**

- **Authentication hardening** — No MFA, no bot protection, a weak password policy and a non-atomic lockout counter (SEC-010, SEC-011, SEC-012, SEC-033) together leave accounts exposed to credential stuffing and password guessing.
- **Editorial workflow integrity** — An editor could publish content by omitting the status parameter (SEC-034), bypassing reviewer approval.
- **Browser-side defenses** — CSRF tokens are not properly enforced (SEC-013) and the Content Security Policy still permits inline scripts (SEC-026).
- **Dependency hygiene** — The braces finding (SEC-032) needs lockfile verification; the reported version may already be the patched release.

> **Verdict:** Not ready for production release until the eight unresolved High-severity findings are remediated or formally risk-accepted.

## Findings Register

<details open>
<summary><b>All 34 findings</b></summary>

| ID | Finding | Category | Severity | Status | OWASP |
| :--- | :--- | :--- | :--- | :--- | :---: |
| SEC-001 | Missing or Inadequate Error Pages | Information Disclosure & Logging | Low | Closed | A05 |
| SEC-002 | Server Banner Information Disclosure | Information Disclosure & Logging | Low | Not Fixed | A05 |
| SEC-003 | Missing or Misconfigured Security Headers | Browser & API Security | Medium | Partially Fixed | A05 |
| SEC-004 | Insufficient Referrer and External-Link Controls | Browser & API Security | Medium | Closed | A05 |
| SEC-005 | Vulnerable Third-Party Components | Third-Party Components | Medium | Closed | A06 |
| SEC-006 | Password Policy Requirements Not Enforced | Authentication | Medium | Not Fixed | A07 |
| SEC-007 | Insufficient Input Validation | Input Validation | Low | Open | A03 |
| SEC-008 | Missing Login Redirect Behavior | Authentication | Low | Not Fixed | A07 |
| SEC-009 | Autocomplete Enabled on Username Field | Authentication | Low | Closed | A07 |
| SEC-010 | CAPTCHA or Equivalent Bot Protection Missing | Authentication | **High** | Not Fixed | A07 |
| SEC-011 | Multi-Factor Authentication Not Implemented | Authentication | **High** | Not Fixed | A07 |
| SEC-012 | Insufficient Password Length and Complexity | Authentication | **High** | Not Fixed | A07 |
| SEC-013 | CSRF Token Not Randomized or Properly Enforced | Browser & API Security | **High** | Not Fixed | A01 |
| SEC-014 | Account Lockout Threshold Misconfigured | Authentication | Medium | Closed | A07 |
| SEC-015 | Concurrent Session Restrictions Not Enforced | Session Management | Medium | Closed | A07 |
| SEC-016 | Session Timeout Not Enforced After Inactivity | Session Management | Medium | Closed | A07 |
| SEC-017 | Broken Access Control | Authorization & Workflow | **High** | Closed | A01 |
| SEC-018 | Unnecessary HTTP Methods Enabled | Server Configuration | Medium | Not Fixed | A05 |
| SEC-019 | Web Application Firewall Not Detected | Server Configuration | Medium | Not Fixed | A05 |
| SEC-020 | Regex DoS in Document Importer | Availability | Medium | Closed | A04 |
| SEC-021 | Published Content Deletable Without Reviewer Approval | Authorization & Workflow | **High** | Closed | A01 |
| SEC-022 | Unbounded Snippet and Variable Expansion | Availability | Medium | Closed | A04 |
| SEC-023 | JWT Remained Valid After Logout | Session Management | **Critical** | Closed | A07 |
| SEC-024 | Uploaded Media Accessible Without Authentication | Authorization & Workflow | **High** | Closed | A01 |
| SEC-025 | Backend CORS Policy Allowed Any Origin | Browser & API Security | **High** | Closed | A05 |
| SEC-026 | CSP Allowed Inline Scripts Without Adequate XSS Containment | Browser & API Security | **High** | Partially Fixed | A05 |
| SEC-027 | AI Chat Logged Full User Questions and Answers | Information Disclosure & Logging | Low | Closed | A09 |
| SEC-028 | Markdown Sanitizer Allowed Author-Controlled ID/Name Attributes | Input Validation | Low | Closed | A03 |
| SEC-029 | Single-Session Enforcement Failed Open on Database Errors | Session Management | Low | Closed | A04 |
| SEC-030 | Verbose Backend Errors Exposed to Browser | Information Disclosure & Logging | Low | Closed | A05 |
| SEC-031 | Document Tags Accepted Unvalidated JSON | Input Validation | Medium | Closed | A03 |
| SEC-032 | Braces 3.0.3 Dependency Reported Vulnerable to DoS | Third-Party Components | **High** | Open | A06 |
| SEC-033 | Login Lockout Counter Not Atomic Under Parallel Attempts | Authentication | **High** | Open | A07 |
| SEC-034 | Editor Could Publish by Omitting Status Parameter | Authorization & Workflow | **High** | Open | A01 |

</details>

## OWASP Top 10 (2021) Coverage

| Category | Findings | IDs |
| :--- | :---: | :--- |
| A01:2021 Broken Access Control | 5 | SEC-013, SEC-017, SEC-021, SEC-024, SEC-034 |
| A03:2021 Injection | 3 | SEC-007, SEC-028, SEC-031 |
| A04:2021 Insecure Design | 3 | SEC-020, SEC-022, SEC-029 |
| A05:2021 Security Misconfiguration | 9 | SEC-001, SEC-002, SEC-003, SEC-004, SEC-018, SEC-019, SEC-025, SEC-026, SEC-030 |
| A06:2021 Vulnerable and Outdated Components | 2 | SEC-005, SEC-032 |
| A07:2021 Identification and Authentication Failures | 11 | SEC-006, SEC-008, SEC-009, SEC-010, SEC-011, SEC-012, SEC-014, SEC-015, SEC-016, SEC-023, SEC-033 |
| A09:2021 Security Logging and Monitoring Failures | 1 | SEC-027 |

## Remediation Roadmap

**Immediate — P1** — Remediate before any production release.

- SEC-010 · CAPTCHA or Equivalent Bot Protection Missing
- SEC-011 · Multi-Factor Authentication Not Implemented
- SEC-012 · Insufficient Password Length and Complexity
- SEC-013 · CSRF Token Not Randomized or Properly Enforced
- SEC-026 · CSP Allowed Inline Scripts Without Adequate XSS Containment
- SEC-032 · Braces 3.0.3 Dependency Reported Vulnerable to DoS
- SEC-033 · Login Lockout Counter Not Atomic Under Parallel Attempts
- SEC-034 · Editor Could Publish by Omitting Status Parameter

**Short term — P2** — Remediate within the next release cycle.

- SEC-003 · Missing or Misconfigured Security Headers
- SEC-006 · Password Policy Requirements Not Enforced
- SEC-018 · Unnecessary HTTP Methods Enabled
- SEC-019 · Web Application Firewall Not Detected

**Planned hardening — P3** — Schedule through the hardening backlog.

- SEC-002 · Server Banner Information Disclosure
- SEC-007 · Insufficient Input Validation
- SEC-008 · Missing Login Redirect Behavior

**Regression** — Re-test closed Critical/High findings after every significant change.

- SEC-017 · Broken Access Control
- SEC-021 · Published Content Deletable Without Reviewer Approval
- SEC-023 · JWT Remained Valid After Logout
- SEC-024 · Uploaded Media Accessible Without Authentication
- SEC-025 · Backend CORS Policy Allowed Any Origin

## Repository Contents

| File | Description |
| --- | --- |
| [`CCMS_Security_Assessment_Report.pdf`](CCMS_Security_Assessment_Report.pdf) | Final assessment report (34 findings, 22 pages) |
| [`CCMS_Security_Assessment_Report.docx`](CCMS_Security_Assessment_Report.docx) | Editable Word version of the same report |
| `README.md` | Project overview and summary of results |

## Skills Demonstrated

- Manual web application and API security testing with Burp Suite
- Authentication, session management and access-control testing
- Business-logic and workflow testing (approval and publishing bypasses)
- Browser security controls: CSRF, CORS, CSP and security headers
- Secure source code review and dependency / supply-chain analysis
- Risk rating, OWASP Top 10 mapping and remediation prioritization
- Professional security reporting for technical and management audiences

## Disclaimer

This assessment was performed on a self-hosted lab deployment for learning and portfolio purposes. Findings are described at a level suitable for remediation planning; the report intentionally contains no exploit code or raw evidence. Severity ratings reflect the lab context and may differ in a production deployment.

## Author

**Faizan Ali** — [GitHub](https://github.com/Faizan-Ali-code)
