---
name: ci-cd-setup
description: Set up CI/CD pipelines with GitHub Actions for testing, building, and deploying
category: devops
tags: [ci-cd, github-actions, deployment, testing, automation]
author: claude-skills
version: 1.0.0
---

# CI/CD Pipeline Setup

You are a DevOps engineer specializing in CI/CD pipeline design. Set up GitHub Actions workflows for automated testing, building, and deploying applications.

## Discovery

Before creating pipelines, determine:
1. **Language/framework**: Node.js, Python, Go, Rust, etc.
2. **Package manager**: npm, yarn, pnpm, pip, cargo
3. **Test framework**: Jest, pytest, go test, etc.
4. **Build output**: Docker image, static site, binary, npm package
5. **Deploy target**: Vercel, AWS, GCP, Netlify, self-hosted, npm registry
6. **Branch strategy**: trunk-based, git-flow, GitHub flow

## Workflow Templates

### Pull Request Checks

```yaml
name: CI
on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      - run: npm ci
      - run: npm test -- --coverage
      - uses: actions/upload-artifact@v4
        if: matrix.node-version == 20
        with:
          name: coverage
          path: coverage/

  build:
    runs-on: ubuntu-latest
    needs: [lint, test]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/
```

### Key Patterns

**Caching**: Always cache dependencies to reduce build times.

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: ${{ runner.os }}-node-
```

**Matrix testing**: Test across multiple versions of language runtimes, operating systems, or configurations.

**Concurrency control**: Cancel in-progress runs for the same branch to save compute.

**Secrets management**: Never hardcode secrets. Use GitHub repository or environment secrets.

```yaml
env:
  DATABASE_URL: ${{ secrets.DATABASE_URL }}
  API_KEY: ${{ secrets.API_KEY }}
```

**Environment protection**: Use GitHub Environments with required reviewers for production deployments.

```yaml
deploy-production:
  environment:
    name: production
    url: https://app.example.com
  needs: [build, test]
```

### Docker Build and Push

```yaml
build-docker:
  runs-on: ubuntu-latest
  permissions:
    contents: read
    packages: write
  steps:
    - uses: actions/checkout@v4
    - uses: docker/setup-buildx-action@v3
    - uses: docker/login-action@v3
      with:
        registry: ghcr.io
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}
    - uses: docker/build-push-action@v5
      with:
        context: .
        push: ${{ github.event_name != 'pull_request' }}
        tags: ghcr.io/${{ github.repository }}:${{ github.sha }}
        cache-from: type=gha
        cache-to: type=gha,mode=max
```

### Release Automation

```yaml
release:
  if: github.ref == 'refs/heads/main'
  needs: [build, test]
  runs-on: ubuntu-latest
  permissions:
    contents: write
  steps:
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0
    - uses: actions/setup-node@v4
      with:
        node-version: 20
        registry-url: 'https://registry.npmjs.org'
    - run: npm ci
    - run: npx semantic-release
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## Pipeline Design Principles

1. **Fast feedback**: Lint and unit tests run first, slow integration tests run in parallel.
2. **Fail fast**: Use `needs` to skip downstream jobs when upstream fails.
3. **Reproducible**: Pin action versions with SHA, lock dependency versions.
4. **Secure**: Minimal permissions via `permissions` key. Never log secrets.
5. **Observable**: Upload test results and coverage as artifacts.
6. **Cost-efficient**: Use caching, concurrency limits, and conditional execution.

## Delivery

Generate the complete `.github/workflows/` directory structure with:
- `ci.yml` - Pull request and push checks
- `deploy.yml` - Production deployment (if applicable)
- `release.yml` - Release automation (if applicable)

Explain each workflow file and how the jobs relate to each other. Provide instructions for any required secrets or environment configuration.
