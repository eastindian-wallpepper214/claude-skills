# Contributing to Claude Skills

Thanks for wanting to contribute! Here's how to add your own skills.

## Adding a New Skill

### 1. Pick a category

| Category | Directory | What belongs here |
|----------|-----------|-------------------|
| Marketing | `skills/marketing/` | SEO, copywriting, ads, email, social, CRO |
| Development | `skills/development/` | Code quality, testing, debugging, tooling |
| Productivity | `skills/productivity/` | Documents, presentations, planning, communication |
| Data Analysis | `skills/data-analysis/` | CSV, SQL, visualization, reporting |
| Design | `skills/design/` | UI/UX, colors, components, accessibility |
| DevOps | `skills/devops/` | CI/CD, Docker, monitoring, infrastructure |
| System | `skills/system/` | OS optimization, performance tuning, cleanup, maintenance |

Need a new category? Open an issue first.

### 2. Create your skill file

```markdown
---
name: my-skill-name
description: Clear one-line description (under 80 chars)
category: category-name
tags: [3-5, relevant, search, terms]
author: your-github-username
version: 1.0.0
---

# Skill Title

You are a [role]. When the user invokes this skill, you will:

## Step 1: Gather Context
- Ask the user for [required inputs]
- Read [relevant files] if applicable

## Step 2: Analyze
- [What analysis to perform]
- [Frameworks or methods to use]

## Step 3: Output
- [Expected deliverable format]
- [How to present results]
```

### 3. Quality checklist

- [ ] Skill has clear, actionable instructions (not vague)
- [ ] Works without external API keys or paid services
- [ ] Follows the frontmatter format exactly
- [ ] Name uses kebab-case (e.g., `my-skill-name`)
- [ ] Description is under 80 characters
- [ ] 3-5 relevant tags for searchability
- [ ] Tested in Claude Code before submitting

### 4. Update catalog.json

Add your skill to `catalog.json`:

```json
{
  "name": "my-skill-name",
  "description": "Clear one-line description",
  "category": "category-name",
  "tags": ["tag1", "tag2"],
  "path": "skills/category-name/my-skill-name.md"
}
```

### 5. Submit a PR

- Fork this repo
- Create a branch: `git checkout -b add-my-skill`
- Add your skill file + update catalog.json
- Submit a PR with a clear description

## Guidelines

- **Be specific** — "Analyze SEO" is too vague. "Check meta tags, heading hierarchy, schema markup, and Core Web Vitals" is good.
- **Be practical** — Skills should produce actionable output, not just information.
- **No API keys** — Skills should work out of the box without requiring external accounts.
- **Test first** — Run your skill in Claude Code before submitting.

## Code of Conduct

Be respectful, constructive, and helpful. We're all here to make Claude Code more useful.
