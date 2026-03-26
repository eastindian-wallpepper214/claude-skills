---
name: git-cleanup
description: Git repository cleanup for large files, stale branches, commit hygiene, and gitignore
category: development
tags: [git, cleanup, branches, gitignore, repository]
author: claude-skills
version: 1.0.0
---

# Git Repository Cleanup

Audit and clean up a git repository. Address large files, stale branches, messy history, and missing gitignore rules.

## Execution Steps

### 1. Repository Health Check

Run an initial assessment:

```bash
# Repo size
du -sh .git
# Total commits
git rev-list --count HEAD
# Number of branches (local and remote)
git branch -a | wc -l
# Largest files in history
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | awk '/^blob/ {print $3, $4}' | sort -rn | head -20
# Check for common large file patterns
git ls-files | grep -iE '\.(zip|tar|gz|jar|exe|dll|bin|iso|mp4|mov|psd|ai)$'
```

### 2. Identify Large Files

Flag files that should not be in the repository:
- Binary files over 1MB (images, videos, archives, compiled binaries)
- Database dumps or data files
- Build artifacts (dist/, build/, node_modules/ committed by accident)
- Vendored dependencies that should use a package manager

For each large file, recommend:
- Remove and add to `.gitignore` if not needed in repo
- Move to Git LFS if the file must be version-controlled
- Use a package manager or artifact storage instead

### 3. Audit .gitignore

Check that `.gitignore` includes rules for:
- `node_modules/`, `vendor/`, `venv/`, `.venv/`
- Build output: `dist/`, `build/`, `out/`, `*.o`, `*.pyc`
- Environment files: `.env`, `.env.local`, `.env.*.local`
- IDE files: `.idea/`, `.vscode/` (except shared settings), `*.swp`
- OS files: `.DS_Store`, `Thumbs.db`
- Log files: `*.log`, `logs/`
- Coverage: `coverage/`, `.nyc_output/`

If `.gitignore` is missing or incomplete, generate a proper one based on the project's language and framework.

### 4. Clean Up Stale Branches

Identify branches that can be removed:

```bash
# Branches already merged into main/master
git branch --merged main
# Remote branches deleted upstream
git remote prune origin --dry-run
# Branches with no commits in 90+ days
git for-each-ref --sort=committerdate --format='%(committerdate:short) %(refname:short)' refs/heads/
```

For each stale branch:
- If merged: safe to delete (`git branch -d branch-name`)
- If unmerged but old: confirm with user before deleting
- Prune remote tracking references: `git remote prune origin`

### 5. Commit Message Hygiene

Audit recent commit messages:

```bash
git log --oneline -50
```

Flag issues:
- Empty or single-word messages ("fix", "update", "wip")
- Messages that do not describe the change
- Inconsistent formatting (some use conventional commits, some do not)

Recommend a commit message convention if none exists:
```
type(scope): short description

- feat: new feature
- fix: bug fix
- refactor: code change that neither fixes nor adds
- docs: documentation only
- test: adding or fixing tests
- chore: build, CI, dependencies
```

### 6. Clean Up Working Directory

Check for:
- Untracked files that should be committed or gitignored
- Modified files that are accidentally unstaged
- Submodule issues (detached HEAD, outdated references)

```bash
git status
git submodule status
```

### 7. Optimize Repository

If the repository is large:

```bash
# Garbage collection
git gc --aggressive --prune=now
# Verify integrity
git fsck --full
# Check new size
du -sh .git
```

### 8. Output Report

```
## Git Cleanup Report: [repository]

### Repository Stats
- Size: X MB (before) -> Y MB (after)
- Commits: N
- Branches: X local, Y remote

### Large Files Found
- path/to/file.zip (15MB) — Recommendation: remove + gitignore
- path/to/data.sql (8MB) — Recommendation: move to LFS

### Branches Cleaned
- Deleted N merged branches
- Pruned N stale remote references
- Flagged N unmerged old branches for review

### .gitignore Updates
- Added N rules for [language/framework]

### Actions Taken
1. [list of commands executed]
```

### 9. Safety Rules

- NEVER run `git push --force` without explicit user confirmation
- NEVER delete unmerged branches without user confirmation
- NEVER rewrite history on shared branches (main, master, develop)
- Always use `--dry-run` first for destructive operations
- Create a backup tag before any history rewriting: `git tag backup-before-cleanup`
