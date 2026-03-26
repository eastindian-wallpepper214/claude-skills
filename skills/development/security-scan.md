---
name: security-scan
description: Security vulnerability scanner for OWASP Top 10, secrets, injection, and dependency risks
category: development
tags: [security, owasp, vulnerabilities, xss, injection]
author: claude-skills
version: 1.0.0
---

# Security Vulnerability Scan

Perform a comprehensive security audit of the codebase. Check for OWASP Top 10 vulnerabilities, hardcoded secrets, dependency risks, and common attack vectors.

## Execution Steps

### 1. Scan for Hardcoded Secrets

Search the entire codebase for:
- API keys, tokens, passwords in source files (regex: `(?i)(api[_-]?key|secret|password|token|credential)\s*[:=]\s*['"][^'"]+['"]`)
- Private keys or certificates committed to the repository
- `.env` files tracked by git (check `.gitignore`)
- Connection strings with embedded credentials
- AWS access keys (pattern: `AKIA[0-9A-Z]{16}`)
- JWT secrets or signing keys in source

**Severity: CRITICAL** for any secret found in tracked files.

### 2. OWASP A01 — Broken Access Control

Check for:
- Missing authentication middleware on protected routes
- Authorization checks that only run client-side
- Direct object reference without ownership validation (`/api/users/:id` — does it check the caller owns that ID?)
- Missing CORS configuration or overly permissive (`Access-Control-Allow-Origin: *` on authenticated endpoints)
- Exposed admin endpoints without role checks

### 3. OWASP A02 — Cryptographic Failures

Check for:
- Passwords stored as plaintext or with weak hashing (MD5, SHA1 without salt)
- Sensitive data transmitted without TLS
- Weak random number generation for tokens (`Math.random()` instead of `crypto.randomBytes`)
- Missing encryption for PII at rest
- Hardcoded encryption keys or IVs

### 4. OWASP A03 — Injection

Scan for:
- **SQL Injection**: String concatenation in SQL queries instead of parameterized queries or ORM methods
- **NoSQL Injection**: Unvalidated user input passed directly to MongoDB `$where` or similar
- **Command Injection**: User input passed to `exec()`, `spawn()`, `system()` without sanitization
- **LDAP Injection**: Unescaped input in LDAP queries
- **Template Injection**: User input rendered in server-side templates without escaping

### 5. OWASP A07 — Cross-Site Scripting (XSS)

Check for:
- User input rendered as `innerHTML` or `dangerouslySetInnerHTML` without sanitization
- Reflected input in server-rendered pages without escaping
- DOM manipulation with unsanitized data (`document.write`, `eval`)
- Missing Content-Security-Policy headers
- SVG or HTML file uploads that could contain scripts

### 6. Dependency Vulnerabilities

Run dependency audit:
- `npm audit` / `yarn audit` for Node.js projects
- `pip audit` or `safety check` for Python projects
- Check for known vulnerable versions in lock files
- Flag dependencies with no maintenance (no updates in 2+ years)
- Check for typosquatting risk on unusual package names

### 7. Input Validation

Verify that every user-facing input is validated:
- Request body schemas enforced (zod, joi, class-validator)
- Query parameters have type checking and bounds
- File uploads check type, size, and content (not just extension)
- Path parameters validated before database lookups
- Headers used in logic are validated

### 8. Authentication and Session Management

Check for:
- Session tokens with sufficient entropy (minimum 128 bits)
- Secure cookie flags (HttpOnly, Secure, SameSite)
- Token expiration and refresh rotation
- Rate limiting on login endpoints
- Account lockout after repeated failures
- Logout actually invalidates the session server-side

### 9. Output Format

```
## Security Scan: [project name]

### CRITICAL (immediate action required)
- [SECRET] path/file.ts:XX — Hardcoded API key found
- [INJECTION] path/file.ts:XX — SQL built with string concatenation

### HIGH (fix before next deploy)
- [XSS] path/file.tsx:XX — Unsanitized input in dangerouslySetInnerHTML
- [AUTH] path/routes.ts:XX — Admin endpoint missing role check

### MEDIUM (fix soon)
- [DEPS] package.json — 3 dependencies with known vulnerabilities
- [CRYPTO] path/auth.ts:XX — Using Math.random() for token generation

### LOW (improve when possible)
- [HEADERS] Missing X-Content-Type-Options header
- [VALIDATION] path/controller.ts:XX — Query param not type-checked

### Summary
- Critical: N, High: N, Medium: N, Low: N
- Recommendation: [SAFE TO DEPLOY / FIX CRITICAL FIRST / BLOCK DEPLOY]
```

### 10. Remediation

For each CRITICAL and HIGH finding, provide:
- The exact vulnerable code
- The fixed code with explanation
- A reference to the relevant OWASP guideline or CWE
