---
name: github-workflow-automation
version: 1.0.0
category: github
description: Advanced GitHub Actions workflow automation with AI swarm coordination, intelligent CI/CD pipelines, and comprehensive repository management
tags:
  - github
  - github-actions
  - ci-cd
  - workflow-automation
  - swarm-coordination
  - deployment
  - security
authors:
  - claude-flow
requires:
  - gh (GitHub CLI)
  - git
  - claude-flow@alpha
  - node (v16+)
---

# GitHub Workflow Automation Skill

## Overview

This skill provides comprehensive GitHub Actions automation with AI swarm coordination. It integrates intelligent CI/CD pipelines, workflow orchestration, and repository management to create self-organizing, adaptive GitHub workflows.

## Quick Start

### Initialize GitHub Workflow Automation
```bash
npx ruv-swarm actions generate-workflow \
  --analyze-codebase \
  --detect-languages \
  --create-optimal-pipeline
```

### Common Commands
```bash
# Optimize existing workflow
npx ruv-swarm actions optimize \
  --workflow ".github/workflows/ci.yml" \
  --suggest-parallelization

# Analyze failed runs
gh run view <run-id> --json jobs,conclusion | \
  npx ruv-swarm actions analyze-failure \
    --suggest-fixes
```

---

## Swarm-Powered GitHub Modes

### gh-coordinator
GitHub workflow orchestration and coordination with hierarchical mode, batch optimization, and up to 10 parallel operations.

### pr-manager
Pull request management with automated multi-reviewer coordination and intelligent conflict resolution.

### ci-orchestrator
CI/CD pipeline coordination with parallel test execution and smart caching.

### security-guardian
Automated security scanning, continuous compliance checks, and proactive vulnerability management.

---

## Workflow Templates

### 1. Intelligent CI with Swarms
```yaml
# .github/workflows/swarm-ci.yml
name: Intelligent CI with Swarms
on: [push, pull_request]

jobs:
  swarm-analysis:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Analyze Changes
        run: |
          npx ruv-swarm actions analyze \
            --commit ${{ github.sha }} \
            --suggest-tests \
            --optimize-pipeline
```

### 2. Adaptive Security Scanning
```yaml
name: Intelligent Security Scan
on:
  schedule:
    - cron: '0 0 * * *'

jobs:
  security-swarm:
    runs-on: ubuntu-latest
    steps:
      - name: Security Analysis Swarm
        run: |
          SECURITY_ISSUES=$(npx ruv-swarm actions security \
            --deep-scan \
            --format json)

          echo "$SECURITY_ISSUES" | jq -r '.issues[]? | @base64' | while read -r issue; do
            gh issue create \
              --title "$(_jq '.title')" \
              --body "$(_jq '.body')" \
              --label "security,critical"
          done
```

### 3. Self-Healing Pipeline
```yaml
name: Self-Healing Pipeline
on: workflow_run

jobs:
  heal-pipeline:
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    runs-on: ubuntu-latest
    steps:
      - name: Diagnose and Fix
        run: |
          npx ruv-swarm actions self-heal \
            --run-id ${{ github.event.workflow_run.id }} \
            --auto-fix-common \
            --create-pr-complex
```

### 4. PR Validation Swarm
```yaml
name: PR Validation Swarm
on: pull_request

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Multi-Agent Validation
        run: |
          PR_DATA=$(gh pr view ${{ github.event.pull_request.number }} --json files,labels)

          RESULTS=$(npx ruv-swarm actions pr-validate \
            --spawn-agents "linter,tester,security,docs" \
            --parallel \
            --pr-data "$PR_DATA")

          gh pr comment ${{ github.event.pull_request.number }} \
            --body "$RESULTS"
```

---

## Monitoring & Analytics

```bash
# Analyze workflow performance
npx ruv-swarm actions analytics \
  --workflow "ci.yml" \
  --period 30d \
  --identify-bottlenecks \
  --suggest-improvements

# Optimize GitHub Actions costs
npx ruv-swarm actions cost-optimize \
  --analyze-usage \
  --suggest-caching \
  --recommend-self-hosted
```

---

## Best Practices

### Workflow Organization
- Use reusable workflows for common patterns
- Implement proper dependency caching
- Set appropriate timeouts
- Define clear job dependencies

### Security
- Store configurations securely using GitHub Secrets
- Use OIDC authentication where possible
- Implement least-privilege permissions
- Audit all swarm operations

### Performance
- Cache dependencies aggressively
- Use appropriate runner sizes
- Implement early termination on failure
- Optimize parallel execution

---

## Command Reference

### Workflow Generation
```bash
npx ruv-swarm actions generate-workflow [options]
  --analyze-codebase       Analyze repository structure
  --detect-languages       Detect programming languages
  --create-optimal-pipeline Generate optimized workflow
```

### Optimization
```bash
npx ruv-swarm actions optimize [options]
  --workflow <path>        Path to workflow file
  --suggest-parallelization Suggest parallel execution
  --reduce-redundancy      Remove redundant steps
```

### Security
```bash
npx ruv-swarm actions security [options]
  --deep-scan             Deep security analysis
  --format <format>       Output format (json/text)
  --create-issues         Auto-create GitHub issues
```

---

## Resources
- [GitHub CLI Docs](https://cli.github.com/manual/)
- [GitHub Actions](https://docs.github.com/en/actions)
- [Claude-Flow](https://github.com/ruvnet/claude-flow)
- [Ruv-Swarm](https://github.com/ruvnet/ruv-swarm)

---

**Skill Status**: Production Ready
**Last Updated**: 2025-01-19
**Maintainer**: claude-flow team
