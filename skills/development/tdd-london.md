---
name: tdd-london
description: TDD London School (mock-first) with red-green-refactor cycle and dependency injection
category: development
tags: [tdd, testing, mocking, dependency-injection, london-school]
author: claude-skills
version: 1.0.0
---

# TDD London School

Guide the implementation of a feature using London School TDD (outside-in, mock-first). Work through the full red-green-refactor cycle, starting with tests, then writing minimal production code.

## Execution Steps

### 1. Clarify the Unit Under Test

Ask or determine:
- What module, class, or function is being built?
- What are its collaborators (dependencies it talks to)?
- What is the public API contract (inputs, outputs, side effects)?

Define the boundary: the unit under test owns its logic; collaborators are mocked.

### 2. Set Up Test Infrastructure

- Identify the test framework in use (Jest, Vitest, Mocha, pytest, etc.) by checking package.json or config files
- Create the test file in the `/tests` directory, mirroring the source path
- Import the mocking utilities (jest.mock, vi.mock, unittest.mock, etc.)
- Set up describe/context blocks reflecting the feature requirements

### 3. Design Collaborator Interfaces First

Before writing any test, define the interfaces (TypeScript) or protocols (Python) for each collaborator:

```typescript
// Define what the unit NEEDS, not what exists yet
interface UserRepository {
  findById(id: string): Promise<User | null>;
  save(user: User): Promise<void>;
}

interface EmailService {
  sendWelcome(email: string): Promise<void>;
}
```

This is the core London School principle: design the collaboration contracts through the tests.

### 4. Red Phase — Write a Failing Test

Write one test case that:
- Creates mock implementations of all collaborators
- Injects mocks via constructor or function parameters (never import mocking)
- Calls the public method under test
- Asserts on the return value AND verifies interactions with mocks
- Verify the test FAILS by running it (the production code does not exist yet)

```typescript
it('should register user and send welcome email', async () => {
  const mockRepo = { findById: vi.fn(), save: vi.fn() };
  const mockEmail = { sendWelcome: vi.fn() };
  const service = new UserService(mockRepo, mockEmail);

  await service.register({ name: 'Alice', email: 'a@b.com' });

  expect(mockRepo.save).toHaveBeenCalledWith(
    expect.objectContaining({ name: 'Alice', email: 'a@b.com' })
  );
  expect(mockEmail.sendWelcome).toHaveBeenCalledWith('a@b.com');
});
```

Run the test. Confirm it fails with the expected error (missing module/function).

### 5. Green Phase — Write Minimal Production Code

- Create the source file in `/src`
- Accept collaborators through constructor injection
- Write the absolute minimum code to make the failing test pass
- Do NOT add logic for cases not yet covered by tests
- Run the test. Confirm it passes.

### 6. Refactor Phase

With the test green:
- Extract helper methods if logic is duplicating
- Improve naming for clarity
- Simplify conditionals
- Ensure the code reads as clean documentation of intent
- Run the test again. Confirm it still passes.

### 7. Repeat the Cycle

For each additional behavior:
1. Write a new failing test (Red)
2. Make it pass with minimal code (Green)
3. Clean up (Refactor)

Cover these cases in order:
- Happy path (primary success scenario)
- Validation failures (invalid input)
- Collaborator failures (repository throws, service unavailable)
- Edge cases (empty collections, boundary values, nulls)

### 8. Verify Coverage

After all cycles, run the test suite and check:
- All tests pass
- Each public method has at least one happy-path and one error test
- Mock interactions verify the unit delegates correctly
- No production code exists without a corresponding test

### 9. Final Checklist

- [ ] All collaborators are injected, never hard-imported in the unit
- [ ] Tests verify behavior (what), not implementation details (how)
- [ ] Each test has a single reason to fail
- [ ] Test names describe the scenario in plain language
- [ ] No test depends on another test's state
- [ ] Mocks are reset between tests (beforeEach or equivalent)
