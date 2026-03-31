---
name: github-release-management
version: 2.0.0
description: Comprehensive GitHub release orchestration with AI swarm coordination for automated versioning, testing, deployment, and rollback management
category: github
tags: [release, deployment, versioning, automation, ci-cd, swarm, orchestration]
author: Claude Flow Team
requires:
  - gh (GitHub CLI)
  - claude-flow
  - ruv-swarm (optional for enhanced coordination)
dependencies:
  - git
  - npm or yarn
  - node >= 20.0.0
---

# GitHub Release Management Skill

Intelligent release automation and orchestration using AI swarms for comprehensive software releases - from changelog generation to multi-platform deployment with rollback capabilities.

## Quick Start

### Simple Release Flow
```bash
# Plan and create a release
gh release create v2.0.0 \
  --draft \
  --generate-notes \
  --title "Release v2.0.0"

# Orchestrate with swarm
npx claude-flow github release-create \
  --version "2.0.0" \
  --build-artifacts \
  --deploy-targets "npm,docker,github"
```

---

## Core Capabilities

### 1. Release Planning & Version Management
- Semantic version analysis and suggestion
- Breaking change detection from commits
- Release timeline generation
- Multi-package version coordination

### 2. Automated Testing & Validation
- Multi-stage test orchestration
- Cross-platform compatibility testing
- Performance regression detection
- Security vulnerability scanning

### 3. Build & Deployment Orchestration
- Multi-platform build coordination
- Parallel artifact generation
- Progressive deployment strategies
- Automated rollback mechanisms

### 4. Documentation & Communication
- Automated changelog generation
- Release notes with categorization
- Migration guide creation
- Stakeholder notification

---

## Basic Usage

### Create Release Draft
```bash
LAST_TAG=$(gh release list --limit 1 --json tagName -q '.[0].tagName')
CHANGELOG=$(gh api repos/:owner/:repo/compare/${LAST_TAG}...HEAD \
  --jq '.commits[].commit.message')

gh release create v2.0.0 \
  --draft \
  --title "Release v2.0.0" \
  --notes "$CHANGELOG" \
  --target main
```

### Basic Version Bump
```bash
npm version patch  # or minor, major
git push --follow-tags
```

---

## Swarm Coordination

### Release Agent Specializations

#### Changelog Agent
```bash
PRS=$(gh pr list --state merged --base main --json number,title,labels,author,mergedAt \
  --jq ".[] | select(.mergedAt > \"$(gh release view v1.0.0 --json publishedAt -q .publishedAt)\")")

npx claude-flow github changelog \
  --prs "$PRS" \
  --from v1.0.0 \
  --to HEAD \
  --categorize \
  --add-migration-guide
```

#### Build Agent
```bash
npx claude-flow github release-build \
  --platforms "linux,macos,windows" \
  --architectures "x64,arm64" \
  --parallel \
  --optimize-size
```

#### Deploy Agent
```bash
npx claude-flow github release-deploy \
  --targets "npm,docker,github,s3" \
  --staged-rollout \
  --monitor-metrics \
  --auto-rollback
```

---

## Progressive Deployment

### Staged Rollout Configuration
```yaml
# .github/release-deployment.yml
deployment:
  strategy: progressive
  stages:
    - name: canary
      percentage: 5
      duration: 1h
      metrics:
        - error-rate < 0.1%
        - latency-p99 < 200ms
      auto-advance: true
    - name: partial
      percentage: 25
      duration: 4h
      approval: qa-team
    - name: full
      percentage: 100
      approval: release-manager
      rollback-enabled: true
```

---

## Hotfix Emergency Procedures

```bash
npx claude-flow github emergency-release \
  --issue 789 \
  --severity critical \
  --target-version v1.2.4 \
  --cherry-pick-commits \
  --fast-track \
  --notify-all
```

---

## GitHub Actions Integration

### Complete Release Workflow
```yaml
# .github/workflows/release.yml
name: Intelligent Release Workflow
on:
  push:
    tags: ['v*']

jobs:
  release-orchestration:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      packages: write
      issues: write

    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - uses: actions/setup-node@v3
        with:
          node-version: '20'
          cache: 'npm'

      - name: Build and Validate
        run: |
          npm ci
          npm run lint
          npm run test:all
          npm run build

      - name: Create GitHub Release
        run: |
          gh release edit ${{ github.ref_name }} \
            --notes "$(cat RELEASE_CHANGELOG.md)" \
            --draft=false

          for file in dist/*; do
            gh release upload ${{ github.ref_name }} "$file"
          done
```

---

## Best Practices

### Release Planning
- **Weekly**: Patch releases with bug fixes
- **Bi-weekly**: Minor releases with features
- **Quarterly**: Major releases with breaking changes
- **On-demand**: Hotfixes for critical issues

### Automation Recommendations
- Automated testing at every stage
- Security scanning before release
- Performance benchmarking
- Progressive deployment with monitoring

---

## Release Checklist Template

### Pre-Release
- [ ] Version numbers updated across all packages
- [ ] Changelog generated and reviewed
- [ ] Breaking changes documented with migration guide
- [ ] All tests passing (unit, integration, e2e)
- [ ] Security scan completed with no critical issues
- [ ] Documentation updated

### Post-Release
- [ ] Release announcement published
- [ ] Monitoring dashboards reviewed
- [ ] Error rates within normal range
- [ ] User feedback collected

---

## Resources
- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [Semantic Versioning Spec](https://semver.org/)
- [Claude Flow Documentation](https://github.com/ruvnet/claude-flow)

---

**Version**: 2.0.0
**Last Updated**: 2025-10-19
**Maintained By**: Claude Flow Team
