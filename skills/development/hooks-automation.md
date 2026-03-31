---
name: Hooks Automation
description: Automated coordination, formatting, and learning from Claude Code operations using intelligent hooks with MCP integration. Includes pre/post task hooks, session management, Git integration, memory coordination, and neural pattern training for enhanced development workflows.
---

# Hooks Automation

Intelligent automation system that coordinates, validates, and learns from Claude Code operations through hooks integrated with MCP tools and neural pattern training.

## What This Skill Does

This skill provides a comprehensive hook system that automatically manages development operations, coordinates swarm agents, maintains session state, and continuously learns from coding patterns.

**Key Capabilities:**
- **Pre-Operation Hooks**: Validate, prepare, and auto-assign agents before operations
- **Post-Operation Hooks**: Format, analyze, and train patterns after operations
- **Session Management**: Persist state, restore context, generate summaries
- **Memory Coordination**: Synchronize knowledge across swarm agents
- **Git Integration**: Automated commit hooks with quality verification
- **Neural Training**: Continuous learning from successful patterns

## Prerequisites

**Required:**
- Claude Flow CLI installed (`npm install -g claude-flow@alpha`)
- Claude Code with hooks enabled
- `.claude/settings.json` with hook configurations

## Quick Start

### Initialize Hooks System

```bash
npx claude-flow init --hooks
```

### Basic Hook Usage

```bash
# Pre-task hook (auto-spawns agents)
npx claude-flow hook pre-task --description "Implement authentication"

# Post-edit hook (auto-formats and stores in memory)
npx claude-flow hook post-edit --file "src/auth.js" --memory-key "auth/login"

# Session end hook (saves state and metrics)
npx claude-flow hook session-end --session-id "dev-session" --export-metrics
```

---

## Available Hooks

### Pre-Operation Hooks

**pre-edit** - Validate and assign agents before file modifications
```bash
npx claude-flow hook pre-edit --file "src/auth/login.js" --auto-assign-agent --validate-syntax
```

**pre-bash** - Check command safety and resource requirements
```bash
npx claude-flow hook pre-bash --command "docker build ." --estimate-resources
```

**pre-task** - Auto-spawn agents and prepare for complex tasks
```bash
npx claude-flow hook pre-task --description "Implement user authentication" --auto-spawn-agents --load-memory
```

### Post-Operation Hooks

**post-edit** - Auto-format, validate, and update memory
```bash
npx claude-flow hook post-edit --file "api/auth.js" --memory-key "auth/login" --auto-format --train-patterns
```

**post-task** - Performance analysis and decision storage
```bash
npx claude-flow hook post-task --task-id "auth-implementation" --analyze-performance --store-decisions
```

### Session Hooks

**session-start** - Initialize new session
```bash
npx claude-flow hook session-start --session-id "dev-session" --load-context
```

**session-end** - Cleanup and persist session state
```bash
npx claude-flow hook session-end --session-id "dev-session" --export-metrics --generate-summary
```

---

## Configuration

### Basic Configuration

Edit `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "^(Write|Edit|MultiEdit)$",
        "hooks": [{
          "type": "command",
          "command": "npx claude-flow hook pre-edit --file '${tool.params.file_path}'"
        }]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "^(Write|Edit|MultiEdit)$",
        "hooks": [{
          "type": "command",
          "command": "npx claude-flow hook post-edit --file '${tool.params.file_path}' --auto-format --train-patterns"
        }]
      }
    ]
  }
}
```

---

## Git Integration

### Pre-Commit Hook
```bash
#!/bin/bash
FILES=$(git diff --cached --name-only --diff-filter=ACM)

for FILE in $FILES; do
  npx claude-flow hook pre-edit --file "$FILE" --validate-syntax
  if [ $? -ne 0 ]; then
    echo "Validation failed for $FILE"
    exit 1
  fi
  npx claude-flow hook post-edit --file "$FILE" --auto-format
done

npm test
exit $?
```

### Pre-Push Hook
```bash
#!/bin/bash
npm run test:all

npx claude-flow hook session-end \
  --generate-report \
  --export-metrics

TRUTH_SCORE=$(npx claude-flow metrics score --format json | jq -r '.truth_score')

if (( $(echo "$TRUTH_SCORE < 0.95" | bc -l) )); then
  echo "Truth score below threshold: $TRUTH_SCORE < 0.95"
  exit 1
fi

exit 0
```

---

## Agent Coordination Workflow

```bash
# Agent 1: Backend Developer
npx claude-flow hook pre-task --description "Implement user authentication API" --auto-spawn-agents --load-memory
npx claude-flow hook pre-edit --file "api/auth.js" --auto-assign-agent
# ... code changes ...
npx claude-flow hook post-edit --file "api/auth.js" --memory-key "swarm/backend/auth-api" --train-patterns
npx claude-flow hook notify --message "Auth API implementation complete" --broadcast

# Agent 2: Test Engineer (receives notification, reads memory)
npx claude-flow hook session-restore --session-id "swarm-current" --restore-memory
npx claude-flow hook pre-task --description "Write tests for auth API" --load-memory
npx claude-flow hook post-edit --file "api/auth.test.js" --memory-key "swarm/testing/auth-api-tests"
```

---

## Memory Coordination Protocol

All hooks follow a three-phase protocol: STATUS -> PROGRESS -> COMPLETE

```javascript
// Phase 1: STATUS
mcp__claude-flow__memory_usage {
  action: "store",
  key: "swarm/hooks/pre-edit/status",
  namespace: "coordination",
  value: JSON.stringify({ status: "running", hook: "pre-edit", file: "src/auth.js" })
}

// Phase 2: PROGRESS
// ... hook processing ...

// Phase 3: COMPLETE
mcp__claude-flow__memory_usage {
  action: "store",
  key: "swarm/hooks/pre-edit/complete",
  namespace: "coordination",
  value: JSON.stringify({ status: "complete", result: "success", agent_assigned: "backend-dev" })
}
```

---

## Performance Tips

1. **Keep Hooks Lightweight** - Target < 100ms execution time
2. **Use Async for Heavy Operations** - Don't block the main flow
3. **Cache Aggressively** - Store frequently accessed data
4. **Batch Related Operations** - Combine multiple actions
5. **Use Memory Wisely** - Set appropriate TTLs

---

## Troubleshooting

### Hooks Not Executing
- Verify `.claude/settings.json` syntax
- Check hook matcher patterns
- Enable debug mode
- Ensure claude-flow CLI is in PATH

### Hook Timeouts
- Increase timeout values in configuration
- Make hooks asynchronous for heavy operations
- Optimize hook logic

---

## Best Practices

1. Configure hooks early during project initialization
2. Use memory keys with clear namespaces
3. Enable auto-formatting for code consistency
4. Train patterns continuously for improvement
5. Monitor performance and track hook execution times

## Related Skills

- **SPARC Methodology** - Hooks enhance SPARC workflows
- **Pair Programming** - Automated quality in pairing sessions
- **GitHub Workflows** - Git integration for commits/PRs
- **Swarm Advanced** - Multi-agent coordination via hooks
