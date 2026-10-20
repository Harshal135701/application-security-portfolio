# Application Security Labs

A practical application security portfolio documenting web security testing, API vulnerability research, and hands-on work in intentionally vulnerable lab environments.

This repository contains my PortSwigger Web Security Academy write-ups and my ongoing security assessment of OWASP crAPI. Each write-up focuses on understanding the vulnerability, reproducing the behavior, documenting evidence, evaluating potential impact, and recommending remediation.

## Repository Structure

```text
PortSwigger-Repo/
├── AppSec-Writeups/
│   ├── Api-Testing/
│   ├── Business-Logic-Vulnerability/
│   ├── CORS/
│   ├── CSRF/
│   ├── File-Upload-Vulnerability/
│   ├── Http-Request-Smuggling/
│   ├── JWT/
│   ├── Oauth2.0/
│   ├── Os-Command-Injection/
│   ├── Path-Traversal/
│   ├── SQL-Injection/
│   ├── Server-Side-Request-Forgery/
│   └── XXE-Vulnerability/
│
└── crAPI-Assessment/
    ├── README.md
    └── findings/
        ├── Finding-01-IDOR/
        └── Finding-02-Weak-Authentication-Reset-Password/
```

## Areas Covered

### Web Application Security

- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- Cross-Origin Resource Sharing (CORS)
- Authentication and access control
- JSON Web Token (JWT) security
- File upload vulnerabilities
- Path traversal
- Server-Side Request Forgery (SSRF)
- XML External Entity (XXE)
- OS command injection
- HTTP request smuggling

### API Security

- API request and response analysis
- Insecure Direct Object References (IDOR) and Broken Object Level Authorization (BOLA)
- Authentication and password-reset workflows
- OTP verification and rate-limiting controls
- Business logic testing
- API versioning and security configuration

### Authentication and Authorization

- OAuth 2.0 and OpenID Connect (OIDC)
- JWT-based authentication
- Access control testing
- Session and authentication workflow analysis

## OWASP crAPI Assessment

The `crAPI-Assessment/` directory contains findings from hands-on testing of OWASP crAPI in a local lab environment.

The assessment focuses on API security, authorization checks, authentication workflows, and the handling of sensitive information.

### Current Findings

**Finding 01 — IDOR in Vehicle Service Reports**

Testing indicated that modifying a service report identifier could return information associated with another record. Cross-user authorization and the full scope of exposure require verification.

[View Finding 01](crAPI-Assessment/findings/Finding-01-IDOR/)

**Finding 02 — Weak Authentication Controls in Password Reset**

Testing indicated that changing the OTP verification endpoint from API version `v3` to `v2` allowed different OTP attempt behavior and enabled a password reset in the local test environment.

[View Finding 02](crAPI-Assessment/findings/Finding-02-Weak-Authentication-Reset-Password/)

These findings are documented separately with testing steps, observed behavior, potential impact, and remediation recommendations. Remediation and retesting status will be updated as the assessment progresses.

## Tools and Environment

- **Burp Suite** — Intercepting and analyzing HTTP requests and responses.
- **Browser Developer Tools** — Inspecting application behavior and network traffic.
- **OWASP crAPI** — Practicing API security testing in a deliberately vulnerable application.
- **Parrot Security OS** — Local security testing environment.
- **Git and GitHub** — Version control and technical documentation.

## Documentation Approach

Write-ups follow a consistent structure where applicable:

- Finding overview and summary
- Testing methodology
- Observed versus expected behavior
- Security impact
- Suspected root cause
- Recommended remediation
- Evidence and screenshots
- Retesting status
- Lessons learned

The goal is to explain not only how a vulnerability was identified, but also why it matters and how developers can address it.

## Learning Goals

- Strengthen practical web and API security testing skills.
- Understand authentication, authorization, and business logic weaknesses.
- Improve vulnerability analysis and reporting.
- Build experience in secure code review.
- Develop a foundation in DevSecOps and application security automation.

## Disclaimer

This repository is intended for educational purposes and authorized security testing. The crAPI assessment is conducted in a local lab environment.

All testing should be performed only on systems for which permission has been granted.

## About

I am pursuing a B.E. in Computer Engineering and developing practical skills in Application Security, API Security, and secure software development.

This repository reflects my ongoing hands-on learning, security research, and preparation for Application Security internship opportunities.