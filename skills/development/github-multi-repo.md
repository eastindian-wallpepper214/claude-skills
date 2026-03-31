---
name: github-multi-repo
version: 1.0.0
description: Multi-repository coordination, synchronization, and architecture management with AI swarm orchestration
category: github-integration
tags: [multi-repo, synchronization, architecture, coordination, github]
author: Claude Flow Team
requires:
  - ruv-swarm@^1.0.11
  - gh-cli@^2.0.0
capabilities:
  - cross-repository coordination
  - package synchronization
  - architecture optimization
  - template management
  - distributed workflows
---

# GitHub Multi-Repository Coordination Skill

## Overview

Advanced multi-repository coordination system that combines swarm intelligence, package synchronization, and repository architecture optimization. This skill enables organization-wide automation, cross-project collaboration, and scalable repository management.

## Core Capabilities

### Multi-Repository Swarm Coordination
Cross-repository AI swarm orchestration for distributed development workflows.

### Package Synchronization
Intelligent dependency resolution and version alignment across multiple packages.

### Repository Architecture
Structure optimization and template management for scalable projects.

### Integration Management
Cross-package integration testing and deployment coordination.

## Quick Start

### Initialize Multi-Repo Coordination
```bash
# Basic swarm initialization
npx claude-flow skill run github-multi-repo init \
  --repos "org/frontend,org/backend,org/shared" \
  --topology hierarchical

# Advanced initialization with synchronization
npx claude-flow skill run github-multi-repo init \
  --repos "org/frontend,org/backend,org/shared" \
  --topology mesh \
  --shared-memory \
  --sync-strategy eventual
```

### Synchronize Packages
```bash
npx claude-flow skill run github-multi-repo sync \
  --packages "claude-code-flow,ruv-swarm" \
  --align-versions \
  --update-docs
```

### Optimize Architecture
```bash
npx claude-flow skill run github-multi-repo optimize \
  --analyze-structure \
  --suggest-improvements \
  --create-templates
```

## Features

### 1. Cross-Repository Swarm Orchestration

#### Repository Discovery
```javascript
// Auto-discover related repositories with gh CLI
const REPOS = Bash(`gh repo list my-organization --limit 100 \
  --json name,description,languages,topics \
  --jq '.[] | select(.languages | keys | contains(["TypeScript"]))'`)

// Analyze repository dependencies
const DEPS = Bash(`gh repo list my-organization --json name | \
  jq -r '.[].name' | while read -r repo; do
    gh api repos/my-organization/$repo/contents/package.json \
      --jq '.content' 2>/dev/null | base64 -d | jq '{name, dependencies}'
  done | jq -s '.'`)
```

#### Synchronized Operations
```bash
# Execute task across repositories
cat /tmp/repos.txt | while read -r repo; do
  gh repo clone org/$repo /tmp/$repo -- --depth=1
  cd /tmp/$repo

  # Apply changes
  npm update
  npm test

  # Create PR if successful
  if [ $? -eq 0 ]; then
    git checkout -b update-dependencies-$(date +%Y%m%d)
    git add -A
    git commit -m "chore: Update dependencies"
    git push origin HEAD
    gh pr create --title "Update dependencies" --body "Automated update" --label "dependencies"
  fi
done
```

### 2. Package Synchronization

#### Version Alignment
```bash
# Synchronize package dependencies and versions
gh api repos/:owner/:repo/git/refs \
  -f ref='refs/heads/sync/package-alignment' \
  -f sha=$(gh api repos/:owner/:repo/git/refs/heads/main --jq '.object.sha')
```

### 3. Orchestration Workflows

#### Dependency Management
```bash
# Create tracking issue
TRACKING_ISSUE=$(gh issue create \
  --title "Dependency Update: typescript@5.0.0" \
  --body "Tracking TypeScript update across all repositories" \
  --label "dependencies,tracking" \
  --json number -q .number)

# Find all TypeScript repositories and update
TS_REPOS=$(gh repo list org --limit 100 --json name | \
  jq -r '.[].name' | while read -r repo; do
    if gh api repos/org/$repo/contents/package.json 2>/dev/null | \
       jq -r '.content' | base64 -d | grep -q '"typescript"'; then
      echo "$repo"
    fi
  done)
```

#### Security Updates
```bash
# Scan all repositories
gh repo list org --limit 100 --json name | jq -r '.[].name' | \
  while read -r repo; do
    gh repo clone org/$repo /tmp/$repo -- --depth=1
    cd /tmp/$repo
    npm audit --json > /tmp/audit-$repo.json
  done
```

## Configuration

### Multi-Repo Config File
```yaml
# .swarm/multi-repo.yml
version: 1
organization: my-org

repositories:
  - name: frontend
    url: github.com/my-org/frontend
    role: ui
    agents: [coder, designer, tester]

  - name: backend
    url: github.com/my-org/backend
    role: api
    agents: [architect, coder, tester]

  - name: shared
    url: github.com/my-org/shared
    role: library
    agents: [analyst, coder]

coordination:
  topology: hierarchical
  communication: webhook

dependencies:
  - from: frontend
    to: [backend, shared]
  - from: backend
    to: [shared]
```

## Synchronization Patterns

### 1. Eventually Consistent
```javascript
{
  "sync": {
    "strategy": "eventual",
    "max-lag": "5m",
    "retry": { "attempts": 3, "backoff": "exponential" }
  }
}
```

### 2. Strong Consistency
```javascript
{
  "sync": {
    "strategy": "strong",
    "consensus": "raft",
    "quorum": 0.51,
    "timeout": "30s"
  }
}
```

## Use Cases

### 1. Microservices Coordination
```bash
npx claude-flow skill run github-multi-repo microservices \
  --services "auth,users,orders,payments" \
  --ensure-compatibility \
  --sync-contracts \
  --integration-tests
```

### 2. Library Updates
```bash
npx claude-flow skill run github-multi-repo lib-update \
  --library "org/shared-lib" \
  --version "2.0.0" \
  --find-consumers \
  --update-imports \
  --run-tests
```

### 3. Organization-Wide Changes
```bash
npx claude-flow skill run github-multi-repo org-policy \
  --policy "add-security-headers" \
  --repos "org/*" \
  --validate-compliance \
  --create-reports
```

## Best Practices

### 1. Repository Organization
- Clear repository roles and boundaries
- Consistent naming conventions
- Documented dependencies
- Shared configuration standards

### 2. Security
- Secure cross-repo authentication
- Encrypted communication channels
- Audit trail for all operations
- Principle of least privilege

### 3. Version Management
- Semantic versioning alignment
- Dependency compatibility validation
- Automated version bump coordination

## Troubleshooting

### Connectivity Issues
```bash
npx claude-flow skill run github-multi-repo diagnose-connectivity \
  --test-all-repos \
  --check-permissions \
  --verify-webhooks
```

### Performance Bottlenecks
```bash
npx claude-flow skill run github-multi-repo perf-analysis \
  --profile-operations \
  --identify-bottlenecks \
  --suggest-optimizations
```

## Support and Resources

- Documentation: https://github.com/ruvnet/claude-flow
- Issues: https://github.com/ruvnet/claude-flow/issues

---

**Version:** 1.0.0
**Last Updated:** 2025-10-19
**Maintainer:** Claude Flow Team
