---
name: debug
description: Systematic debugging using hypothesis-driven investigation and root cause analysis
category: development
tags: [debugging, troubleshooting, root-cause, diagnostics, logging]
author: claude-skills
version: 1.0.0
---

# Systematic Debugging

Apply a structured, hypothesis-driven debugging process to find and fix the root cause of a reported issue. Avoid random changes; work methodically.

## Execution Steps

### 1. Capture the Bug Report

Gather these details (ask if not provided):
- **Expected behavior**: What should happen?
- **Actual behavior**: What happens instead? (error message, wrong output, crash)
- **Reproduction steps**: Exact sequence to trigger the issue
- **Environment**: OS, runtime version, browser, deployment target
- **Frequency**: Always, intermittent, or one-time?
- **Recent changes**: What was deployed or modified before the issue appeared?

### 2. Reproduce the Issue

Before investigating code, confirm you can trigger the bug:
- Follow the reproduction steps exactly
- Check logs, console output, network responses for error details
- If the bug is intermittent, identify conditions that increase likelihood (load, timing, specific input)
- If you cannot reproduce it, gather more context before proceeding

Record the exact error message, stack trace, or incorrect output.

### 3. Form Hypotheses

Based on the symptoms, list 2-5 possible causes ranked by likelihood:

```
Hypothesis 1 (most likely): [description] — because [evidence]
Hypothesis 2: [description] — because [evidence]
Hypothesis 3: [description] — because [evidence]
```

Common hypothesis categories:
- **Input handling**: Bad data, missing validation, encoding issues
- **State management**: Stale cache, race condition, uninitialized variable
- **Integration**: API contract changed, timeout, network failure
- **Logic error**: Off-by-one, wrong operator, inverted condition
- **Environment**: Missing env var, wrong config, version mismatch

### 4. Binary Search the Problem Space

Narrow down the location using divide-and-conquer:
- If the system has layers (controller > service > repository), test each boundary
- Add temporary logging at the midpoint of the suspected code path
- Determine: does the correct data arrive at this point? If yes, the bug is downstream. If no, it is upstream.
- Repeat, halving the search space each time

Tools to use:
- `console.log` / `logger.debug` with labeled output: `DEBUG[service.process]: input=...`
- Debugger breakpoints if available
- Network inspector for HTTP issues
- Database query logs for data issues

### 5. Isolate the Root Cause

Once narrowed to a specific function or line:
- Read the code carefully, tracing the data flow manually
- Check edge cases: null, undefined, empty array, zero, negative numbers
- Check async ordering: is an await missing? Could a promise resolve out of order?
- Check state mutations: is something modifying shared state unexpectedly?
- Check recent git blame/log for that section: what changed and when?

Confirm the root cause by predicting: "If I change X, the bug should disappear." Test that prediction.

### 6. Verify It Is the Root Cause (Not a Symptom)

Ask:
- Does fixing this explain ALL observed symptoms?
- Could this same root cause produce issues elsewhere?
- Is there a deeper underlying problem (wrong architecture, missing abstraction)?

If fixing the suspected cause only partially resolves symptoms, there may be multiple issues. Track them separately.

### 7. Implement the Fix

- Write a test that fails with the current code and passes with the fix (regression test)
- Make the minimal change to fix the root cause
- Do not fix unrelated things in the same change
- Run the full test suite to confirm no regressions

### 8. Clean Up

- Remove all temporary debug logging
- Update or add documentation if the fix reveals a non-obvious behavior
- If the bug was caused by missing validation, add validation at the system boundary
- If the bug was caused by unclear code, refactor for clarity

### 9. Post-Mortem Summary

Document findings:

```
## Bug Report: [title]

### Symptoms
[What was observed]

### Root Cause
[Specific code location and explanation of why it failed]

### Fix
[What was changed and why]

### Regression Test
[Test name/file that prevents recurrence]

### Prevention
[What process or pattern change would catch this class of bug earlier]
```

### 10. Prevention Patterns

Based on the bug class, recommend:
- **Input bugs**: Add schema validation at API boundaries
- **State bugs**: Use immutable data structures or state machines
- **Async bugs**: Use structured concurrency, avoid fire-and-forget
- **Integration bugs**: Add contract tests, health checks, circuit breakers
- **Logic bugs**: Increase unit test coverage for edge cases
