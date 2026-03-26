---
name: code-review
description: Thorough code review checking bugs, security, performance, readability, and SOLID principles
category: development
tags: [review, quality, security, solid, best-practices]
author: claude-skills
version: 1.0.0
---

# Code Review

Perform a thorough, multi-pass code review on the specified file(s) or diff. Produce categorized findings with severity levels.

## Execution Steps

### 1. Gather Context

Read the target file(s) or git diff. Identify the language, framework, and surrounding architecture. Check for existing tests and documentation.

### 2. Pass 1 — Correctness and Bugs

Examine the code for:
- Off-by-one errors, null/undefined dereferences, unhandled edge cases
- Race conditions in async code (missing await, unguarded shared state)
- Incorrect boolean logic or inverted conditions
- Resource leaks (unclosed handles, missing cleanup in finally blocks)
- Type mismatches or unsafe casts
- Dead code or unreachable branches

### 3. Pass 2 — Security

Check for:
- User input used without validation or sanitization
- SQL injection (string concatenation in queries instead of parameterized)
- XSS vectors (unsanitized output rendered in HTML)
- Hardcoded secrets, API keys, or credentials
- Path traversal (unsanitized file paths from user input)
- Insecure deserialization or eval usage
- Missing authentication or authorization checks
- Overly permissive CORS or CSP headers

### 4. Pass 3 — Performance

Identify:
- N+1 query patterns (loops issuing individual DB calls)
- Unbounded data fetching (missing LIMIT, no pagination)
- Synchronous blocking on the main thread or event loop
- Unnecessary re-computation (missing memoization for expensive pure functions)
- Large objects copied in hot loops instead of passed by reference
- Missing indexes implied by query patterns

### 5. Pass 4 — Readability and Maintainability

Evaluate:
- Function and variable naming clarity
- Function length (flag anything over 30 lines)
- File length (flag anything over 500 lines)
- Nesting depth (flag deeper than 3 levels)
- Magic numbers or strings without named constants
- Missing or misleading comments
- Consistent formatting and code style

### 6. Pass 5 — SOLID Principles and Design

Assess:
- **S** — Single Responsibility: Does each class/module do one thing?
- **O** — Open/Closed: Can behavior be extended without modifying existing code?
- **L** — Liskov Substitution: Can subtypes replace their base types safely?
- **I** — Interface Segregation: Are interfaces focused or bloated?
- **D** — Dependency Inversion: Do modules depend on abstractions, not concretions?
- Also check for DRY violations (duplicated logic that should be extracted)

### 7. Output Format

Present findings in this structure:

```
## Code Review: [filename]

### Critical (must fix before merge)
- [SECURITY] Line XX: Description of issue and recommended fix
- [BUG] Line XX: Description of issue and recommended fix

### Warning (should fix)
- [PERF] Line XX: Description and suggestion
- [SOLID] Line XX: Description and suggestion

### Info (consider improving)
- [READABILITY] Line XX: Description and suggestion
- [STYLE] Line XX: Description and suggestion

### Summary
- Total findings: N (X critical, Y warning, Z info)
- Recommendation: APPROVE / REQUEST CHANGES / BLOCK
```

### 8. Positive Feedback

Also note 1-3 things done well. Good patterns should be reinforced, not just problems flagged.

### 9. Offer Fixes

For each Critical and Warning finding, provide a concrete code snippet showing the recommended fix. Use the Edit tool to apply fixes if the user confirms.
