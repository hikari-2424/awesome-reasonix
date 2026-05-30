---
name: strategic-compact
description: Suggests manual context compaction at logical intervals to preserve context through task phases rather than arbitrary auto-compaction.
---

# Strategic Compact Skill

Suggests manual `/compact` at strategic points in your workflow rather than relying on arbitrary auto-compaction.

## When to Activate

- Running long sessions that approach context limits (200K+ tokens)
- Working on multi-phase tasks (research 鈫?plan 鈫?implement 鈫?test)
- Switching between unrelated tasks within the same session
- After completing a major milestone and starting new work
- When responses slow down or become less coherent (context pressure)

## Why Strategic Compaction?

Auto-compaction triggers at arbitrary points:
- Often mid-task, losing important context
- No awareness of logical task boundaries
- Can interrupt complex multi-step operations

Strategic compaction at logical boundaries:
- **After exploration, before execution** 鈥?Compact research context, keep implementation plan
- **After completing a milestone** 鈥?Fresh start for next phase
- **Before major context shifts** 鈥?Clear exploration context before different task

## How It Works

The `suggest-compact.js` script runs on PreToolUse (Edit/Write) and:

1. **Tracks tool calls** 鈥?Counts tool invocations in session
2. **Threshold detection** 鈥?Suggests at configurable threshold (default: 50 calls)
3. **Periodic reminders** 鈥?Reminds every 25 calls after threshold

## Hook Setup

Add to your `~/.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [{ "type": "command", "command": "node ~/.claude/skills/strategic-compact/suggest-compact.js" }]
      },
      {
        "matcher": "Write",
        "hooks": [{ "type": "command", "command": "node ~/.claude/skills/strategic-compact/suggest-compact.js" }]
      }
    ]
  }
}
```

## Configuration

Environment variables:
- `COMPACT_THRESHOLD` 鈥?Tool calls before first suggestion (default: 50)

## Compaction Decision Guide

Use this table to decide when to compact:

| Phase Transition | Compact? | Why |
|-----------------|----------|-----|
| Research 鈫?Planning | Yes | Research context is bulky; plan is the distilled output |
| Planning 鈫?Implementation | Yes | Plan is in TodoWrite or a file; free up context for code |
| Implementation 鈫?Testing | Maybe | Keep if tests reference recent code; compact if switching focus |
| Debugging 鈫?Next feature | Yes | Debug traces pollute context for unrelated work |
| Mid-implementation | No | Losing variable names, file paths, and partial state is costly |
| After a failed approach | Yes | Clear the dead-end reasoning before trying a new approach |

## What Survives Compaction

Understanding what persists helps you compact with confidence:

| Persists | Lost |
|----------|------|
| CLAUDE.md instructions | Intermediate reasoning and analysis |
| TodoWrite task list | File contents you previously read |
| Memory files (`~/.claude/memory/`) | Multi-step conversation context |
| Git state (commits, branches) | Tool call history and counts |
| Files on disk | Nuanced user preferences stated verbally |

## Best Practices

1. **Compact after planning** 鈥?Once plan is finalized in TodoWrite, compact to start fresh
2. **Compact after debugging** 鈥?Clear error-resolution context before continuing
3. **Don't compact mid-implementation** 鈥?Preserve context for related changes
4. **Read the suggestion** 鈥?The hook tells you *when*, you decide *if*
5. **Write before compacting** 鈥?Save important context to files or memory before compacting
6. **Use `/compact` with a summary** 鈥?Add a custom message: `/compact Focus on implementing auth middleware next`

## Token Optimization Patterns

### Trigger-Table Lazy Loading
Instead of loading full skill content at session start, use a trigger table that maps keywords to skill paths. Skills load only when triggered, reducing baseline context by 50%+:

| Trigger | Skill | Load When |
|---------|-------|-----------|
| "test", "tdd", "coverage" | tdd-workflow | User mentions testing |
| "security", "auth", "xss" | security-review | Security-related work |
| "deploy", "ci/cd" | deployment-patterns | Deployment context |

### Context Composition Awareness
Monitor what's consuming your context window:
- **CLAUDE.md files** 鈥?Always loaded, keep lean
- **Loaded skills** 鈥?Each skill adds 1-5K tokens
- **Conversation history** 鈥?Grows with each exchange
- **Tool results** 鈥?File reads, search results add bulk

### Duplicate Instruction Detection
Common sources of duplicate context:
- Same rules in both `~/.claude/rules/` and project `.claude/rules/`
- Skills that repeat CLAUDE.md instructions
- Multiple skills covering overlapping domains

### Context Optimization Tools
- `token-optimizer` MCP 鈥?Automated 95%+ token reduction via content deduplication
- `context-mode` 鈥?Context virtualization (315KB to 5.4KB demonstrated)

## Related

- [The Longform Guide](https://x.com/affaanmustafa/status/2014040193557471352) 鈥?Token optimization section
- Memory persistence hooks 鈥?For state that survives compaction
- `continuous-learning` skill 鈥?Extracts patterns before session ends
