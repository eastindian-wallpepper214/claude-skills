---
name: "Skill Builder"
description: "Create new Claude Code Skills with proper YAML frontmatter, progressive disclosure structure, and complete directory organization. Use when you need to build custom skills for specific workflows, generate skill templates, or understand the Claude Skills specification."
---

# Skill Builder

## What This Skill Does

Creates production-ready Claude Code Skills with proper YAML frontmatter, progressive disclosure architecture, and complete file/folder structure. This skill guides you through building skills that Claude can autonomously discover and use across all surfaces (Claude.ai, Claude Code, SDK, API).

## Prerequisites

- Claude Code 2.0+ or Claude.ai with Skills support
- Basic understanding of Markdown and YAML
- Text editor or IDE

## Quick Start

### Creating Your First Skill

```bash
# 1. Create skill directory (MUST be at top level, NOT in subdirectories!)
mkdir -p ~/.claude/skills/my-first-skill

# 2. Create SKILL.md with proper format
cat > ~/.claude/skills/my-first-skill/SKILL.md << 'EOF'
---
name: "My First Skill"
description: "Brief description of what this skill does and when Claude should use it. Maximum 1024 characters."
---

# My First Skill

## What This Skill Does
[Your instructions here]

## Quick Start
[Basic usage]
EOF

# 3. Verify skill is detected
# Restart Claude Code or refresh Claude.ai
```

---

## Complete Specification

### YAML Frontmatter (REQUIRED)

Every SKILL.md **must** start with YAML frontmatter containing exactly two required fields:

```yaml
---
name: "Skill Name"                    # REQUIRED: Max 64 chars
description: "What this skill does    # REQUIRED: Max 1024 chars
and when Claude should use it."       # Include BOTH what & when
---
```

**`name`** (REQUIRED):
- Max Length: 64 characters
- Format: Human-friendly display name, Title Case
- Examples: "API Documentation Generator", "React Component Builder"

**`description`** (REQUIRED):
- Max Length: 1024 characters
- MUST include: 1) What the skill does 2) When Claude should invoke it
- Front-load key trigger words, be specific about use cases

**Critical**: Only `name` and `description` are used by Claude. Additional fields are ignored.

---

### Directory Structure

#### Minimal Skill (Required)
```
~/.claude/skills/
  my-skill/
    SKILL.md                     # REQUIRED: Main skill file
```

**IMPORTANT**: Skills MUST be directly under `~/.claude/skills/[skill-name]/`. Claude Code does NOT support nested subdirectories or namespaces.

#### Full-Featured Skill (Recommended)
```
~/.claude/skills/
  my-skill/
    SKILL.md                     # REQUIRED
    README.md                    # Optional: Human-readable docs
    scripts/                     # Optional: Executable scripts
    resources/                   # Optional: Templates, examples, schemas
    docs/                        # Optional: Additional documentation
```

#### Skills Locations

**Personal Skills** (available across all projects):
```
~/.claude/skills/
```

**Project Skills** (team-shared, version controlled):
```
<project-root>/.claude/skills/
```

---

### Progressive Disclosure Architecture

Claude Code uses a 3-level system to scale to 100+ skills without context penalty:

**Level 1: Metadata** - Name + Description loaded at startup for all skills (~200 chars each)
**Level 2: SKILL.md Body** - Main instructions loaded only when skill is triggered (~1-10KB)
**Level 3+: Referenced Files** - Deep reference loaded on-demand as Claude navigates

---

### Content Best Practices

**Front-Load Keywords**:
```yaml
# Good: Keywords first
description: "Generate TypeScript interfaces from JSON schema. Use when converting schemas, creating types, or building API clients."
```

**Include Trigger Conditions**:
```yaml
# Good: Clear "when" clause
description: "Debug React performance issues using Chrome DevTools. Use when components re-render unnecessarily or investigating slow updates."
```

---

### Validation Checklist

**YAML Frontmatter**:
- [ ] Starts and ends with `---`
- [ ] Contains `name` (max 64 chars) and `description` (max 1024 chars)
- [ ] Description includes "what" and "when"

**File Structure**:
- [ ] SKILL.md exists in skill directory
- [ ] Directory is directly in `~/.claude/skills/[skill-name]/`

**Content Quality**:
- [ ] Level 1 (Overview) is brief and clear
- [ ] Level 2 (Quick Start) shows common use case
- [ ] Level 3 (Details) provides step-by-step guide
- [ ] Examples are concrete and runnable

---

## Skill Builder Templates

### Template 1: Basic Skill
```markdown
---
name: "My Basic Skill"
description: "One sentence what. One sentence when to use."
---

# My Basic Skill

## What This Skill Does
[2-3 sentences describing functionality]

## Quick Start
```bash
# Single command to get started
```

## Step-by-Step Guide

### Step 1: Setup
[Instructions]

### Step 2: Usage
[Instructions]

## Troubleshooting
- **Issue**: Problem description
  - **Solution**: Fix description
```

### Template 2: Advanced Skill
```markdown
---
name: "My Advanced Skill"
description: "Comprehensive what with all features. Use when [trigger 1], [trigger 2], or [trigger 3]."
---

# My Advanced Skill

## Overview
[Brief description]

## Prerequisites
- Technology 1
- Technology 2

## Quick Start
```bash
./scripts/quickstart.sh
```

## Step-by-Step Guide
[Detailed instructions]

## Advanced Features
[Complex scenarios]

## Troubleshooting
[Common issues]
```

---

## Learn More

### Official Resources
- [Anthropic Agent Skills Documentation](https://docs.claude.com/en/docs/agents-and-tools/agent-skills)
- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
