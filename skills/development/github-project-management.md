---
name: github-project-management
title: GitHub Project Management
version: 2.0.0
category: github
description: Comprehensive GitHub project management with swarm-coordinated issue tracking, project board automation, and sprint planning
author: Claude Code
tags:
  - github
  - project-management
  - issue-tracking
  - project-boards
  - sprint-planning
  - agile
  - swarm-coordination
difficulty: intermediate
prerequisites:
  - GitHub CLI (gh) installed and authenticated
  - ruv-swarm or claude-flow MCP server configured
  - Repository access permissions
---

# GitHub Project Management

## Overview

A comprehensive skill for managing GitHub projects using AI swarm coordination. This skill combines intelligent issue management, automated project board synchronization, and swarm-based coordination for efficient project delivery.

## Quick Start

### Basic Issue Creation with Swarm Coordination

```bash
# Create a coordinated issue
gh issue create \
  --title "Feature: Advanced Authentication" \
  --body "Implement OAuth2 with social login..." \
  --label "enhancement,swarm-ready"

# Initialize swarm for issue
npx claude-flow@alpha hooks pre-task --description "Feature implementation"
```

### Project Board Quick Setup

```bash
# Get project ID
PROJECT_ID=$(gh project list --owner @me --format json | \
  jq -r '.projects[0].id')

# Initialize board sync
npx ruv-swarm github board-init \
  --project-id "$PROJECT_ID" \
  --sync-mode "bidirectional"
```

---

## Core Capabilities

### 1. Issue Management & Triage

#### Automated Issue Creation

```bash
# Create multiple related issues using gh CLI
gh issue create \
  --title "Feature: Advanced GitHub Integration" \
  --body "Implement comprehensive GitHub workflow automation..." \
  --label "feature,github,high-priority"

gh issue create \
  --title "Bug: Merge conflicts in integration branch" \
  --body "Resolve merge conflicts..." \
  --label "bug,integration,urgent"
```

#### Issue-to-Swarm Conversion

```bash
# Get issue details
ISSUE_DATA=$(gh issue view 456 --json title,body,labels,assignees,comments)

# Create swarm from issue
npx ruv-swarm github issue-to-swarm 456 \
  --issue-data "$ISSUE_DATA" \
  --auto-decompose \
  --assign-agents
```

#### Automated Issue Triage

```bash
# Analyze and triage unlabeled issues
npx ruv-swarm github triage \
  --unlabeled \
  --analyze-content \
  --suggest-labels \
  --assign-priority
```

#### Task Decomposition

```bash
# Get issue body
ISSUE_BODY=$(gh issue view 456 --json body --jq '.body')

# Decompose into subtasks
SUBTASKS=$(npx ruv-swarm github issue-decompose 456 \
  --body "$ISSUE_BODY" \
  --max-subtasks 10 \
  --assign-priorities)
```

### 2. Project Board Automation

#### Board Initialization

```bash
# Get project details
PROJECT_ID=$(gh project list --owner @me --format json | \
  jq -r '.projects[] | select(.title == "Development Board") | .id')

# Initialize swarm with project
npx ruv-swarm github board-init \
  --project-id "$PROJECT_ID" \
  --sync-mode "bidirectional" \
  --create-views "swarm-status,agent-workload,priority"
```

#### Real-time Board Sync

```bash
npx ruv-swarm github board-sync \
  --map-status '{
    "todo": "To Do",
    "in_progress": "In Progress",
    "review": "Review",
    "done": "Done"
  }' \
  --auto-move-cards \
  --update-metadata
```

### 3. Sprint Planning & Tracking

```bash
# Manage sprints with swarms
npx ruv-swarm github sprint-manage \
  --sprint "Sprint 23" \
  --auto-populate \
  --capacity-planning \
  --track-velocity

# Track milestone progress
npx ruv-swarm github milestone-track \
  --milestone "v2.0 Release" \
  --update-board \
  --show-dependencies \
  --predict-completion
```

### 4. Advanced Coordination

#### Multi-Board Synchronization

```bash
npx ruv-swarm github multi-board-sync \
  --boards "Development,QA,Release" \
  --sync-rules '{
    "Development->QA": "when:ready-for-test",
    "QA->Release": "when:tests-pass"
  }'
```

#### Epic Coordination

```bash
npx ruv-swarm github epic-swarm \
  --epic 123 \
  --child-issues "456,457,458" \
  --orchestrate
```

---

## Issue Templates

### Integration Issue Template

```markdown
## Integration Task

### Overview
[Brief description of integration requirements]

### Objectives
- [ ] Component A integration
- [ ] Component B validation
- [ ] Testing and verification
- [ ] Documentation updates

### Swarm Coordination
- **Coordinator**: Overall progress tracking
- **Analyst**: Technical validation
- **Tester**: Quality assurance
- **Documenter**: Documentation updates
```

---

## Best Practices

### 1. Swarm-Coordinated Issue Management
- Always initialize swarm for complex issues
- Assign specialized agents based on issue type
- Use memory for progress coordination
- Regular automated progress updates

### 2. Board Organization
- Clear column definitions with consistent naming
- Systematic labeling strategy across repositories
- Regular board grooming and maintenance

### 3. Data Integrity
- Bidirectional sync validation
- Conflict resolution strategies
- Comprehensive audit trails

---

## Quick Reference Commands

```bash
# Issue Management
gh issue create --title "..." --body "..." --label "..."
npx ruv-swarm github issue-init <number>
npx ruv-swarm github issue-decompose <number>
npx ruv-swarm github triage --unlabeled

# Project Boards
npx ruv-swarm github board-init --project-id <id>
npx ruv-swarm github board-sync
npx ruv-swarm github board-analytics

# Sprint Management
npx ruv-swarm github sprint-manage --sprint "Sprint X"
npx ruv-swarm github milestone-track --milestone "vX.X"

# Analytics
npx ruv-swarm github issue-metrics --issue <number>
npx ruv-swarm github board-kpis
```

---

## Additional Resources

- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [GitHub Projects Documentation](https://docs.github.com/en/issues/planning-and-tracking-with-projects)
- [Swarm Coordination Guide](https://github.com/ruvnet/ruv-swarm)
- [Claude Flow Documentation](https://github.com/ruvnet/claude-flow)

---

**Last Updated**: 2025-10-19
**Version**: 2.0.0
**Maintainer**: Claude Code
