---
name: stream-chain
description: Stream-JSON chaining for multi-agent pipelines, data transformation, and sequential workflows
version: 1.0.0
category: workflow
tags: [streaming, pipeline, chaining, multi-agent, workflow]
---

# Stream-Chain Skill

Execute sophisticated multi-step workflows where each agent's output flows into the next, enabling complex data transformations and sequential processing pipelines.

## Overview

Stream-Chain provides two powerful modes for orchestrating multi-agent workflows:

1. **Custom Chains** (`run`): Execute custom prompt sequences with full control
2. **Predefined Pipelines** (`pipeline`): Use battle-tested workflows for common tasks

Each step in a chain receives the complete output from the previous step, enabling sophisticated multi-agent coordination through streaming data flow.

---

## Quick Start

### Run a Custom Chain

```bash
claude-flow stream-chain run \
  "Analyze codebase structure" \
  "Identify improvement areas" \
  "Generate action plan"
```

### Execute a Pipeline

```bash
claude-flow stream-chain pipeline analysis
```

---

## Custom Chains (`run`)

### Syntax

```bash
claude-flow stream-chain run <prompt1> <prompt2> [...] [options]
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--verbose` | Show detailed execution information | `false` |
| `--timeout <seconds>` | Timeout per step | `30` |
| `--debug` | Enable debug mode with full logging | `false` |

### Examples

#### Basic Development Chain

```bash
claude-flow stream-chain run \
  "Write a user authentication function" \
  "Add input validation and error handling" \
  "Create unit tests with edge cases"
```

#### Security Audit Workflow

```bash
claude-flow stream-chain run \
  "Analyze authentication system for vulnerabilities" \
  "Identify and categorize security issues by severity" \
  "Propose fixes with implementation priority" \
  "Generate security test cases" \
  --timeout 45 \
  --verbose
```

---

## Predefined Pipelines (`pipeline`)

### Available Pipelines

#### 1. Analysis Pipeline
```bash
claude-flow stream-chain pipeline analysis
```
Steps: Structure Analysis -> Issue Detection -> Recommendations

#### 2. Refactor Pipeline
```bash
claude-flow stream-chain pipeline refactor
```
Steps: Candidate Identification -> Prioritization -> Implementation

#### 3. Test Pipeline
```bash
claude-flow stream-chain pipeline test
```
Steps: Coverage Analysis -> Test Design -> Implementation

#### 4. Optimize Pipeline
```bash
claude-flow stream-chain pipeline optimize
```
Steps: Profiling -> Strategy -> Implementation

---

## Custom Pipeline Definitions

Define reusable pipelines in `.claude-flow/config.json`:

```json
{
  "streamChain": {
    "pipelines": {
      "security": {
        "name": "Security Audit Pipeline",
        "description": "Comprehensive security analysis",
        "prompts": [
          "Scan codebase for security vulnerabilities",
          "Categorize issues by severity (critical/high/medium/low)",
          "Generate fixes with priority and implementation steps",
          "Create security test suite"
        ],
        "timeout": 45
      }
    }
  }
}
```

---

## Advanced Use Cases

### Multi-Agent Coordination

```bash
claude-flow stream-chain run \
  "Research best practices for API design" \
  "Design REST API with discovered patterns" \
  "Implement API endpoints with validation" \
  "Generate OpenAPI specification" \
  "Create integration tests" \
  "Write deployment documentation"
```

### Code Migration Workflow

```bash
claude-flow stream-chain run \
  "Analyze legacy codebase dependencies" \
  "Create migration plan with risk assessment" \
  "Generate modernized code for high-priority modules" \
  "Create migration tests" \
  "Document migration steps and rollback procedures"
```

---

## Best Practices

1. **Clear and Specific Prompts** - Be precise about what each step should do
2. **Logical Progression** - Order prompts to build on previous outputs
3. **Appropriate Timeouts** - Simple: 30s, Analysis: 45-60s, Implementation: 60-90s
4. **Verification Steps** - Include validation in your chains
5. **Iterative Refinement** - Use chains for progressive improvement

---

## Integration with Claude Flow

### Combine with Swarm Coordination

```bash
claude-flow swarm init --topology mesh
claude-flow stream-chain run \
  "Agent 1: Research task" \
  "Agent 2: Implement solution" \
  "Agent 3: Test implementation"
```

### Memory Integration

Stream chains automatically store context in memory for cross-session persistence.

---

## Performance Characteristics

- **Throughput**: 2-5 steps per minute (varies by complexity)
- **Context Size**: Up to 100K tokens per step
- **Memory Usage**: ~50MB per active chain
- **Concurrency**: Supports parallel chain execution

---

## Related Skills

- **SPARC Methodology**: Systematic development workflow
- **Swarm Coordination**: Multi-agent orchestration
- **Memory Management**: Persistent context storage
