---
name: Pair Programming
description: AI-assisted pair programming with multiple modes (driver/navigator/switch), real-time verification, quality monitoring, and comprehensive testing. Supports TDD, debugging, refactoring, and learning sessions. Features automatic role switching, continuous code review, security scanning, and performance optimization with truth-score verification.
---

# Pair Programming

Collaborative AI pair programming with intelligent role management, real-time quality monitoring, and comprehensive development workflows.

## What This Skill Does

This skill provides professional pair programming capabilities with AI assistance, supporting multiple collaboration modes, continuous verification, and integrated testing.

**Key Capabilities:**
- **Multiple Modes**: Driver, Navigator, Switch, TDD, Review, Mentor, Debug
- **Real-Time Verification**: Automatic quality scoring with rollback on failures
- **Role Management**: Seamless switching between driver/navigator roles
- **Testing Integration**: Auto-generate tests, track coverage, continuous testing
- **Code Review**: Security scanning, performance analysis, best practice enforcement
- **Session Persistence**: Auto-save, recovery, export, and sharing

## Prerequisites

**Required:**
- Claude Flow CLI installed (`npm install -g claude-flow@alpha`)
- Git repository (optional but recommended)

**Recommended:**
- Testing framework (Jest, pytest, etc.)
- Linter configured (ESLint, pylint, etc.)
- Code formatter (Prettier, Black, etc.)

## Quick Start

### Basic Session
```bash
claude-flow pair --start
```

### TDD Session
```bash
claude-flow pair --start \
  --mode tdd \
  --test-first \
  --coverage 90
```

---

## Available Modes

### Driver Mode
You write code while AI provides guidance.

```bash
claude-flow pair --start --mode driver
```

**Commands:**
```
/suggest     - Get implementation suggestions
/review      - Request code review
/explain     - Ask for explanations
/optimize    - Request optimization ideas
```

### Navigator Mode
AI writes code while you provide direction.

```bash
claude-flow pair --start --mode navigator
```

**Commands:**
```
/implement   - Direct implementation
/refactor    - Request refactoring
/test        - Generate tests
/document    - Add documentation
```

### Switch Mode
Automatically alternates roles at intervals.

```bash
claude-flow pair --start --mode switch --interval 10m
```

### Specialized Modes

**TDD Mode**:
```bash
claude-flow pair --start --mode tdd --test-first --coverage 100
```

**Review Mode**:
```bash
claude-flow pair --start --mode review --strict --security
```

**Mentor Mode**:
```bash
claude-flow pair --start --mode mentor --explain-all --pace slow
```

**Debug Mode**:
```bash
claude-flow pair --start --mode debug --verbose --trace
```

---

## In-Session Commands

### Code Commands
```
/explain [--level basic|detailed|expert]
/suggest [--type refactor|optimize|security|style]
/implement <description>
/refactor [--pattern <pattern>] [--scope function|file|module]
/optimize [--target speed|memory|both]
/document [--format jsdoc|markdown|inline]
```

### Testing Commands
```
/test [--watch] [--coverage] [--only <pattern>]
/test-gen [--type unit|integration|e2e]
/coverage [--report html|json|terminal]
/mock <target> [--realistic]
```

### Review Commands
```
/review [--scope current|file|changes] [--strict]
/security [--deep] [--fix]
/perf [--profile] [--suggestions]
/quality [--detailed]
```

### Git Commands
```
/diff [--staged] [--file <file>]
/commit [--message <msg>] [--amend]
/branch [create|switch|delete|list] [<name>]
```

---

## Configuration

### Basic Configuration
Create `.claude-flow/pair-config.json`:

```json
{
  "pair": {
    "enabled": true,
    "defaultMode": "switch",
    "defaultAgent": "auto",
    "autoStart": false,
    "theme": "professional"
  }
}
```

### Built-in Agents

```json
{
  "agents": {
    "senior-dev": {
      "expertise": ["architecture", "patterns", "optimization"],
      "style": "thorough",
      "reviewLevel": "strict"
    },
    "tdd-specialist": {
      "expertise": ["testing", "mocks", "coverage"],
      "style": "test-first",
      "reviewLevel": "comprehensive"
    },
    "debugger-expert": {
      "expertise": ["debugging", "profiling", "tracing"],
      "style": "analytical",
      "reviewLevel": "focused"
    }
  }
}
```

---

## Real-World Examples

### Feature Implementation
```bash
claude-flow pair --start \
  --mode switch \
  --agent senior-dev \
  --focus implement \
  --verify \
  --test
```

### Bug Fixing
```bash
claude-flow pair --start \
  --mode navigator \
  --agent debugger-expert \
  --focus debug \
  --trace
```

### TDD Session
```bash
claude-flow pair --start \
  --mode tdd \
  --agent tdd-specialist \
  --test-first
```

---

## Quality Metrics

### Truth Score Thresholds
```
Error:   < 0.90
Warning: 0.90 - 0.95
Good:    0.95 - 0.98
Excellent: > 0.98
```

### Coverage Thresholds
```
Error:   < 70%
Warning: 70% - 80%
Good:    80% - 90%
Excellent: > 90%
```

---

## Best Practices

### Session Practices
1. **Clear Goals** - Define session objectives upfront
2. **Appropriate Mode** - Choose based on task type
3. **Enable Verification** - For critical code paths
4. **Regular Testing** - Maintain quality continuously
5. **Regular Breaks** - Take breaks every 45-60 minutes

### Mode Selection
- **Driver Mode**: When learning, controlling implementation
- **Navigator Mode**: For rapid prototyping, generation
- **Switch Mode**: Long sessions, balanced collaboration
- **TDD Mode**: Building with tests
- **Review Mode**: Quality focus
- **Mentor Mode**: Learning priority
- **Debug Mode**: Fixing issues

---

## Related Commands

- `claude-flow pair --help` - Show help
- `claude-flow pair config` - Manage configuration
- `claude-flow pair profile` - Manage profiles
- `claude-flow pair templates` - List templates
- `claude-flow pair agents` - List available agents
