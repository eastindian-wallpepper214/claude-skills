---
name: sparc-methodology
description: SPARC (Specification, Pseudocode, Architecture, Refinement, Completion) comprehensive development methodology with multi-agent orchestration
version: 2.7.0
category: development
tags:
  - sparc
  - tdd
  - architecture
  - orchestration
  - methodology
  - multi-agent
author: Claude Flow
---

# SPARC Methodology - Comprehensive Development Framework

## Overview

SPARC (Specification, Pseudocode, Architecture, Refinement, Completion) is a systematic development methodology integrated with Claude Flow's multi-agent orchestration capabilities. It provides 17 specialized modes for comprehensive software development, from initial research through deployment and monitoring.

## Core Philosophy

- **Systematic Approach**: Structured phases from specification to completion
- **Test-Driven Development**: Tests written before implementation
- **Parallel Execution**: Concurrent agent coordination for 2.8-4.4x speed improvements
- **Memory Integration**: Persistent knowledge sharing across agents and sessions
- **Quality First**: Comprehensive reviews, testing, and validation

### Key Principles

1. **Specification Before Code**: Define requirements and constraints clearly
2. **Design Before Implementation**: Plan architecture and components
3. **Tests Before Features**: Write failing tests, then make them pass
4. **Review Everything**: Code quality, security, and performance checks
5. **Document Continuously**: Maintain current documentation throughout

---

## Development Phases

### Phase 1: Specification
Define requirements, constraints, and success criteria.
**Key Modes**: `researcher`, `analyzer`, `memory-manager`

### Phase 2: Architecture
Design system structure and component interfaces.
**Key Modes**: `architect`, `designer`, `orchestrator`

### Phase 3: Refinement (TDD Implementation)
Implement features with test-first approach.
**Key Modes**: `tdd`, `coder`, `tester`

### Phase 4: Review
Ensure code quality, security, and performance.
**Key Modes**: `reviewer`, `optimizer`, `debugger`

### Phase 5: Completion
Integration, deployment, and monitoring.
**Key Modes**: `workflow-manager`, `documenter`, `memory-manager`

---

## Available Modes

### Core Orchestration Modes

- **`orchestrator`** - Multi-agent task orchestration with TodoWrite/Task/Memory coordination
- **`swarm-coordinator`** - Specialized swarm management for complex multi-agent workflows
- **`workflow-manager`** - Process automation and workflow orchestration
- **`batch-executor`** - Parallel task execution for high-throughput operations

### Development Modes

- **`coder`** - Autonomous code generation with batch file operations
- **`architect`** - System design with Memory-based coordination
- **`tdd`** - Test-driven development with comprehensive testing
- **`reviewer`** - Code review using batch file analysis

### Analysis and Research Modes

- **`researcher`** - Deep research with parallel WebSearch/WebFetch
- **`analyzer`** - Code and data analysis with pattern recognition
- **`optimizer`** - Performance optimization and bottleneck resolution

### Creative and Support Modes

- **`designer`** - UI/UX design with accessibility focus
- **`innovator`** - Creative problem-solving and novel solutions
- **`documenter`** - Comprehensive documentation generation
- **`debugger`** - Systematic debugging and issue resolution
- **`tester`** - Comprehensive testing beyond TDD
- **`memory-manager`** - Knowledge management and context preservation

---

## Activation Methods

### Method 1: MCP Tools (Preferred)

```javascript
mcp__claude-flow__sparc_mode {
  mode: "<mode-name>",
  task_description: "<task description>",
  options: { /* mode-specific options */ }
}
```

### Method 2: NPX CLI

```bash
npx claude-flow sparc run <mode> "task description"
npx claude-flow sparc modes          # List all modes
npx claude-flow sparc tdd "feature"  # TDD workflow
npx claude-flow sparc pipeline "task" # Full pipeline
```

---

## Orchestration Patterns

### Hierarchical Coordination
Best for complex projects with clear delegation hierarchy.

### Mesh Coordination
Best for collaborative tasks requiring peer-to-peer communication.

### Sequential Pipeline
Best for ordered workflow execution (spec -> design -> code -> test -> review).

### Parallel Execution
Best for independent tasks that can run concurrently.

---

## TDD Workflows

### Red-Green-Refactor Cycle

```javascript
// RED: Write failing test
mcp__claude-flow__sparc_mode {
  mode: "tester",
  task_description: "create failing test for shopping cart add item",
  options: { expect_failure: true }
}

// GREEN: Minimal implementation
mcp__claude-flow__sparc_mode {
  mode: "coder",
  task_description: "implement minimal code to pass test",
  options: { minimal: true }
}

// REFACTOR: Improve code quality
mcp__claude-flow__sparc_mode {
  mode: "coder",
  task_description: "refactor shopping cart implementation",
  options: { maintain_tests: true }
}
```

---

## Best Practices

### 1. Memory Integration
Always use Memory for cross-agent coordination.

### 2. Parallel Operations
Batch all related operations in a single message.

### 3. Test Coverage
Maintain minimum 90% coverage.

### 4. File Organization
```
project/
  src/           # Source code
  tests/         # Test files
  docs/          # Documentation
  config/        # Configuration
  scripts/       # Utility scripts
  examples/      # Example code
```

---

## Common Workflows

### Feature Development
```bash
npx claude-flow sparc run researcher "authentication patterns"
npx claude-flow sparc run architect "design auth system"
npx claude-flow sparc tdd "user authentication feature"
npx claude-flow sparc run reviewer "review auth implementation"
npx claude-flow sparc run documenter "document auth API"
```

### Bug Investigation
```bash
npx claude-flow sparc run analyzer "investigate bug #456"
npx claude-flow sparc run debugger "fix memory leak in service X"
npx claude-flow sparc run tester "regression tests for bug #456"
npx claude-flow sparc run reviewer "validate bug fix"
```

### Complete Pipeline
```bash
npx claude-flow sparc pipeline "e-commerce checkout feature"
# Automatically runs: researcher -> architect -> coder -> tdd -> reviewer -> optimizer -> documenter
```

---

## Performance Benefits

- **84.8%** SWE-Bench solve rate
- **32.3%** token reduction through optimizations
- **2.8-4.4x** speed improvement with parallel execution
- **90%+** test coverage standard

---

## Support and Resources

- **Documentation**: https://github.com/ruvnet/claude-flow
- **Issues**: https://github.com/ruvnet/claude-flow/issues
- **NPM Package**: https://www.npmjs.com/package/claude-flow

---

Remember: **SPARC = Systematic, Parallel, Agile, Refined, Complete**
