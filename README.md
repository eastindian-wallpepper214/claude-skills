# ⚡ Claude Skills

> **154+ plug-and-play skills for Claude Code. Install in one command.**

```bash
npx claude-skills install seo-audit
```

Or install an entire pack:

```bash
npx claude-skills install-pack marketing
```

---

## What is this?

**Claude Skills** are pre-built prompts that give Claude Code specialized abilities — SEO audits, code reviews, paid advertising analysis, AI agent orchestration, SPARC methodology, and more. Each skill is a single markdown file that drops into your `.claude/commands/` directory.

Think of it as **an app store for Claude Code**.

## Quick Start

```bash
# Install a single skill
npx claude-skills install code-review

# Install all marketing skills (35 skills)
npx claude-skills install-pack marketing

# Install all advertising skills (18 skills)
npx claude-skills install-pack advertising

# Install everything (154+ skills)
npx claude-skills install-pack all

# Browse available skills
npx claude-skills list

# Search
npx claude-skills search "seo"
```

Then in Claude Code, use the skill:
```
/seo-audit
/code-review
/ads-google
/sparc
```

---

## Skill Catalog

### 📢 Advertising (18 skills)

| Skill | Description |
|-------|-------------|
| `ads` | Full multi-platform audit — Google, Meta, LinkedIn, TikTok, Microsoft |
| `ads-google` | Google Ads deep analysis — 74 checks across Search, PMax, Display, YouTube |
| `ads-meta` | Meta Ads — 46 checks across Pixel/CAPI, creative, Advantage+ |
| `ads-linkedin` | LinkedIn Ads — 25 checks, ABM, lead gen forms, bidding |
| `ads-tiktok` | TikTok Ads — creative quality, Smart+, TikTok Shop |
| `ads-microsoft` | Microsoft/Bing Ads — search, Audience Network, Copilot integration |
| `ads-youtube` | YouTube Ads — skippable, bumper, Shorts, Demand Gen |
| `ads-apple` | Apple Search Ads — campaign structure, bid health, TAP coverage |
| `ads-audit` | Full multi-platform audit with parallel analysis |
| `ads-budget` | Budget allocation and bidding strategy review |
| `ads-competitor` | Competitor ad intelligence — copy, creative, keywords, spend |
| `ads-create` | Campaign concept and copy brief generator |
| `ads-creative` | Cross-platform creative quality audit |
| `ads-dna` | Brand DNA extractor from website |
| `ads-generate` | AI image generation for ad creatives |
| `ads-landing` | Landing page quality and CRO assessment |
| `ads-photoshoot` | Product photography enhancement with AI (5 styles) |
| `ads-plan` | Strategic paid advertising planning and roadmap |

### 🤖 AI Agents (10 skills)

| Skill | Description |
|-------|-------------|
| `agentdb-advanced` | Advanced AgentDB — QUIC sync, multi-database, hybrid search |
| `agentdb-learning` | RL training with 9 algorithms — Q-Learning, Actor-Critic, SARSA |
| `agentdb-memory-patterns` | Persistent memory patterns — session, long-term, context |
| `agentdb-optimization` | AgentDB performance — quantization (4-32x), HNSW (150x faster) |
| `agentdb-vector-search` | Semantic vector search for RAG and document retrieval |
| `reasoningbank-agentdb` | ReasoningBank adaptive learning with AgentDB integration |
| `reasoningbank-intelligence` | Adaptive learning — pattern recognition and meta-cognitive systems |
| `swarm-advanced` | Advanced swarm orchestration patterns for complex workflows |
| `swarm-orchestration` | Multi-agent swarms with dynamic topology and parallel execution |
| `verification-quality` | Truth scoring and automatic rollback with 0.95 accuracy threshold |

### 💻 Development (21 skills)

| Skill | Description |
|-------|-------------|
| `code-review` | Thorough code review — bugs, security, performance, SOLID |
| `tdd-london` | TDD London School — mock-first, red-green-refactor |
| `pair-programming` | AI pair programming — driver/navigator/switch modes, TDD |
| `sparc-methodology` | SPARC development methodology with multi-agent orchestration |
| `github-code-review` | GitHub code review with AI swarm coordination |
| `github-multi-repo` | Multi-repo coordination and architecture management |
| `github-project-management` | GitHub project management — issues, boards, sprint planning |
| `github-release-management` | Automated release coordination and versioning |
| `github-workflow-automation` | GitHub Actions CI/CD with AI swarm coordination |
| `hooks-automation` | Claude Code hooks with pre/post task automation |
| `skill-builder` | Create new Claude Code skills with YAML frontmatter |
| `stream-chain` | Stream-JSON chaining for multi-agent pipelines |
| `browser-automation` | Web browser automation with AI-optimized snapshots |
| `api-design` | REST API design — OpenAPI spec, endpoints, versioning |
| `debug` | Systematic debugging — root cause analysis |
| `docker-setup` | Dockerfile & docker-compose — multi-stage builds |
| `git-cleanup` | Git cleanup — stale branches, large files, gitignore |
| `performance-audit` | Performance audit — N+1, memory leaks, bundle size |
| `refactor` | Code refactoring — smells, patterns, clean code |
| `security-scan` | Security scanner — OWASP Top 10, secrets, injection |
| `typescript-migrate` | JS → TypeScript migration — incremental, types, config |

### 🐙 GitHub (18 skills)

| Skill | Description |
|-------|-------------|
| `pr-manager` | Complete PR lifecycle management |
| `pr-enhance` | AI-generated PR descriptions and reviewer suggestions |
| `code-review-swarm` | Multi-agent code review across security, performance, architecture |
| `issue-tracker` | Intelligent issue management with progress monitoring |
| `issue-triage` | Automated issue triage — categorize, label, assign |
| `release-manager` | Automated release with changelog and deployment |
| `release-swarm` | Complex releases using AI swarms |
| `repo-analyze` | Deep repository analysis — quality, dependencies, tech debt |
| `repo-architect` | Repository structure optimization and multi-repo management |
| `swarm-issue` | Transform GitHub issues into multi-agent tasks |
| `swarm-pr` | PR-based swarm coordination for review and validation |
| `sync-coordinator` | Multi-repo version alignment and dependency sync |
| `workflow-automation` | GitHub Actions with self-healing CI/CD pipelines |
| `multi-repo-swarm` | Cross-repository swarm orchestration |
| `project-board-sync` | Sync AI swarms with GitHub Projects |
| `github-modes` | GitHub integration modes for batch workflow optimization |
| `github-swarm` | GitHub swarm for coordinating agents across repo tasks |
| `github-code-review` | Focused review workflow with automated PR comments |

### 📣 Marketing (35 skills)

| Skill | Description |
|-------|-------------|
| `seo-audit` | Comprehensive SEO audit — technical, on-page, Core Web Vitals |
| `ai-seo` | AI SEO — get cited by LLMs, AEO/GEO optimization |
| `copywriting` | Marketing copy — AIDA framework for landing pages |
| `copy-editing` | Edit and improve existing marketing copy |
| `content-strategy` | 90-day content plan with topic clusters |
| `social-content` | Social media for LinkedIn, Twitter, Instagram |
| `email-sequence` | Email drip campaigns — welcome, nurture, re-engagement |
| `cold-email` | B2B cold email and follow-up sequences |
| `competitor-analysis` | Competitive analysis — positioning, pricing, SWOT |
| `competitor-alternatives` | Competitor comparison and alternative pages for SEO |
| `landing-page-cro` | Landing page CRO — headlines, CTAs, friction |
| `page-cro` | Optimize any marketing page for conversions |
| `form-cro` | Form optimization — reduce friction, increase submissions |
| `signup-flow-cro` | Signup flow optimization — reduce registration dropoff |
| `onboarding-cro` | Post-signup onboarding and user activation |
| `paywall-upgrade-cro` | In-app paywall and upgrade screen optimization |
| `popup-cro` | Popup and modal optimization — exit-intent, email capture |
| `ab-test-setup` | A/B test design with sample size and statistical significance |
| `analytics-tracking` | GA4, GTM, event tracking, UTM setup and audit |
| `schema-markup` | JSON-LD structured data for Google rich results |
| `programmatic-seo` | SEO pages at scale — directory, location, comparison pages |
| `site-architecture` | Site structure, navigation, URL patterns, internal linking |
| `ad-creative` | Ad copy for Google, Meta, LinkedIn with A/B variants |
| `paid-ads` | Paid advertising strategy across all platforms |
| `pricing-strategy` | Pricing — value metrics, packaging, willingness-to-pay |
| `launch-strategy` | Product launch playbook — pre-launch through post-launch |
| `marketing-ideas` | Generate marketing and growth ideas for SaaS |
| `marketing-psychology` | Behavioral economics and persuasion frameworks |
| `lead-magnets` | Create lead magnets — ebooks, checklists, templates |
| `free-tool-strategy` | Plan free tools for lead generation and SEO |
| `referral-program` | Referral and affiliate program design |
| `churn-prevention` | Reduce churn — save offers, dunning, win-back |
| `revops` | Revenue ops — lead scoring, routing, MQL/SQL |
| `sales-enablement` | Sales collateral — decks, one-pagers, objection handling |
| `product-marketing-context` | Product positioning, ICP, and messaging framework |

### ⚡ SPARC (32 skills)

| Skill | Description |
|-------|-------------|
| `sparc` | SPARC Orchestrator — delegate complex workflows across modes |
| `sparc-architect` | System architecture and technical blueprints |
| `sparc-coder` | Implementation with TDD practices |
| `sparc-tdd` | Test-driven development with red-green-refactor |
| `sparc-debugger` | Systematic debugging and root cause analysis |
| `sparc-security-review` | Static and dynamic security audits |
| `sparc-docs-writer` | Markdown documentation for APIs and usage |
| `sparc-devops` | Deployment automation and CI/CD management |
| `sparc-supabase-admin` | Supabase — database, auth, storage, RLS policies |
| `sparc-researcher` | Deep research and technology evaluation |
| `sparc-designer` | UI/UX design and design system creation |
| `sparc-optimizer` | Refactor, modularize, and improve performance |
| `sparc-integration` | Merge all modes into production-ready systems |
| `sparc-mcp` | MCP server and external service orchestration |
| `sparc-orchestrator` | Coordinate multi-step workflows across specialists |
| `sparc-swarm-coordinator` | Multi-agent swarm topology and task distribution |
| `sparc-spec-pseudocode` | Capture requirements, edge cases, and pseudocode |
| `sparc-memory-manager` | Persistent context and cross-agent knowledge |
| `sparc-post-deployment-monitor` | Post-launch performance and log monitoring |
| `sparc-refinement` | Code cleanup, file limits, and deduplication |
| `sparc-reviewer` | Code review and standards compliance |
| `sparc-tester` | Comprehensive testing strategy and generation |
| `sparc-innovator` | Generate novel solutions and creative approaches |
| `sparc-analyzer` | Codebase and system analysis for patterns and issues |
| `sparc-ask` | Task formulation guide for delegating to SPARC modes |
| `sparc-batch-executor` | Run multiple SPARC tasks in sequence or parallel |
| `sparc-code` | Write clean modular code from pseudocode specs |
| `sparc-debug` | Troubleshoot runtime bugs and integration failures |
| `sparc-documenter` | Comprehensive documentation from code and specs |
| `sparc-workflow-manager` | Multi-phase workflow management with checkpoints |
| `sparc-modes` | Overview guide to all SPARC modes |
| `sparc-tutorial` | Onboarding guide for learning SPARC methodology |

### 🛠️ Tools (4 skills)

| Skill | Description |
|-------|-------------|
| `nano-banana` | AI image generation via OpenRouter (Gemini) — text-to-image, editing |
| `claude-flow-help` | Claude Flow command reference — swarm, agents, memory, hooks |
| `claude-flow-memory` | Claude Flow memory — store, search, retrieve with AgentDB |
| `claude-flow-swarm` | Claude Flow swarm coordination — init, topology, multi-agent |

### 📊 Productivity (5 skills)

| Skill | Description |
|-------|-------------|
| `ppt-generator` | PowerPoint generator — charts, consulting style, pptxgenjs |
| `meeting-notes` | Meeting notes processor — action items, decisions, owners |
| `project-planner` | Project planning — milestones, tasks, Gantt timeline |
| `email-writer` | Professional email — cold, follow-up, negotiation |
| `document-analyzer` | Document analysis — PDF summarizer, key info extraction |

### 📈 Data Analysis (4 skills)

| Skill | Description |
|-------|-------------|
| `csv-analyzer` | CSV analysis — statistics, trends, outliers, correlations |
| `sql-generator` | Natural language to SQL — joins, aggregations, window functions |
| `data-visualizer` | Data visualization — bar, line, scatter, heatmap, funnel |
| `report-builder` | Business reports — executive summaries, KPI dashboards |

### 🎨 Design (3 skills)

| Skill | Description |
|-------|-------------|
| `ui-review` | UI/UX review — accessibility, hierarchy, contrast, mobile |
| `color-palette` | Color palettes — brand colors, CSS vars, WCAG contrast |
| `component-library` | UI component scaffolder — React/Vue, props, types, stories |

### ⚙️ DevOps (3 skills)

| Skill | Description |
|-------|-------------|
| `ci-cd-setup` | CI/CD pipelines — GitHub Actions, test/build/deploy |
| `nginx-config` | Nginx config — reverse proxy, SSL, caching, rate limits |
| `monitoring-setup` | App monitoring — health checks, error tracking, alerting |

### 🖥️ System (1 skill)

| Skill | Description |
|-------|-------------|
| `system-optimize` | Windows performance optimizer — disk, startup, memory, network, privacy |

---

## Skill Packs

Install multiple skills at once:

| Pack | Skills | Command |
|------|--------|---------|
| `advertising` | 18 advertising skills | `npx claude-skills install-pack advertising` |
| `ai-agents` | 10 AI agent skills | `npx claude-skills install-pack ai-agents` |
| `development` | 21 dev skills | `npx claude-skills install-pack development` |
| `github` | 18 GitHub skills | `npx claude-skills install-pack github` |
| `marketing` | 35 marketing skills | `npx claude-skills install-pack marketing` |
| `sparc` | 32 SPARC skills | `npx claude-skills install-pack sparc` |
| `productivity` | 5 productivity skills | `npx claude-skills install-pack productivity` |
| `data` | 4 data analysis skills | `npx claude-skills install-pack data` |
| `design` | 3 design skills | `npx claude-skills install-pack design` |
| `devops` | 3 devops skills | `npx claude-skills install-pack devops` |
| `tools` | 4 utility tools | `npx claude-skills install-pack tools` |
| `system` | 1 system skill | `npx claude-skills install-pack system` |
| `all` | All 154+ skills | `npx claude-skills install-pack all` |

---

## Manual Installation

Don't want to use the CLI? Just copy the files:

```bash
# Clone the repo
git clone https://github.com/asinadarsh/claude-skills.git

# Copy the skills you want
cp claude-skills/skills/marketing/seo-audit.md ~/.claude/commands/
```


---

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

**Adding a new skill:**

1. Fork this repo
2. Create your skill file in the appropriate `skills/<category>/` directory
3. Follow the [skill template](#skill-format)
4. Add your skill to `catalog.json`
5. Submit a PR

### Skill Format

```markdown
---
name: my-skill
description: One-line description of what the skill does
category: marketing|development|productivity|data-analysis|design|devops
tags: [relevant, search, terms]
author: your-github-username
version: 1.0.0
---

# My Skill Name

[Detailed prompt instructions that Claude Code will execute]
```

---

## FAQ

**Q: Where do skills get installed?**
A: `~/.claude/commands/` — Claude Code's slash command directory.

**Q: Can I use these with Claude Desktop?**
A: These are designed for Claude Code (CLI/IDE). They won't work in Claude.ai chat.

**Q: How do I uninstall a skill?**
A: `npx claude-skills uninstall <name>` or delete the file from `~/.claude/commands/`.

**Q: Can I modify installed skills?**
A: Yes! They're just markdown files. Edit them however you want.

---

## Star History

If this repo helped you, give it a ⭐ — it helps others find it.

---

## License

MIT — use these skills however you want.
