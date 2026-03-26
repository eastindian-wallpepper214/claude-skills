---
name: refactor
description: Identify code smells and apply refactoring patterns while preserving behavior
category: development
tags: [refactoring, code-smells, clean-code, design-patterns, maintainability]
author: claude-skills
version: 1.0.0
---

# Code Refactoring

Systematically identify code smells in the target file(s) and apply well-known refactoring patterns. The key constraint: behavior must be preserved. Every change must keep existing tests green.

## Execution Steps

### 1. Establish Safety Net

Before any refactoring:
- Run the existing test suite and confirm all tests pass
- If no tests exist, write characterization tests that capture current behavior before touching anything
- Note the test command so it can be re-run after each transformation

### 2. Identify Code Smells

Read the target file(s) and catalog smells from this checklist:

**Size smells:**
- Long method (>30 lines)
- Large class/file (>500 lines)
- Long parameter list (>3 parameters)

**Logic smells:**
- Deeply nested conditionals (>3 levels)
- Switch/case on type (should be polymorphism)
- Duplicated code blocks (DRY violation)
- Feature envy (method uses another object's data more than its own)
- Primitive obsession (using strings/numbers where a value object fits)

**Coupling smells:**
- Shotgun surgery (one change requires edits in many files)
- Inappropriate intimacy (class accesses another's internals)
- Middle man (class that only delegates)
- God object (one class knows/does everything)

**Naming smells:**
- Unclear variable or function names
- Boolean parameters without named meaning
- Comments explaining what code does (the code should say it)

### 3. Prioritize by Impact

Rank identified smells:
1. Bugs likely hiding in complex/duplicated code (fix first)
2. Readability blockers that slow down every future reader
3. Coupling issues that make change harder
4. Style and naming improvements

### 4. Apply Refactoring Patterns

Use the appropriate pattern for each smell. Apply ONE pattern at a time, then re-run tests.

**Extract Method** — Pull a coherent block into a named function.
Use when: long methods, duplicated blocks, or comments marking sections.

**Extract Class** — Move a group of related fields and methods to a new class.
Use when: a class has multiple responsibilities.

**Replace Conditional with Polymorphism** — Create a class hierarchy instead of switch/if chains on type.
Use when: the same condition appears in multiple places.

**Introduce Parameter Object** — Group related parameters into a single object.
Use when: the same group of parameters travels together across functions.

**Replace Magic Numbers/Strings with Constants** — Name every literal.
Use when: numbers or strings appear without explanation.

**Replace Temp with Query** — Turn a temporary variable into a method call.
Use when: a temp is assigned once and used to explain a computation.

**Decompose Conditional** — Extract the condition and each branch into named methods.
Use when: an if/else block has complex conditions or long branches.

**Move Method/Field** — Relocate to the class that uses it most.
Use when: feature envy is detected.

**Inline Method** — Remove a method that just delegates trivially.
Use when: the method body is as clear as the name.

**Guard Clauses** — Replace nested ifs with early returns.
Use when: deep nesting obscures the happy path.

### 5. After Each Transformation

- Run the full test suite
- If any test fails, revert the change immediately and investigate
- Commit the passing refactoring as a small, focused change

### 6. Output Format

Present the refactoring plan before applying changes:

```
## Refactoring Plan: [filename]

### Smell 1: [description] (Line XX-YY)
- Pattern: [Extract Method / Replace Conditional / etc.]
- Change: [what will happen]
- Risk: LOW / MEDIUM

### Smell 2: ...

### Execution Order
1. Smell X (safest, most impactful)
2. Smell Y (depends on X being done)
3. ...
```

### 7. Verify Final State

After all refactorings:
- Run full test suite one final time
- Confirm no behavioral changes (same inputs produce same outputs)
- Check that file length, function length, and nesting depth are all improved
- Summarize metrics: lines removed, functions extracted, complexity reduced
