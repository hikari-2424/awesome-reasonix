---
name: plankton-code-quality
description: "Write-time code quality enforcement using Plankton 鈥?auto-formatting, linting, and Claude-powered fixes on every file edit via hooks."
---

# Plankton Code Quality Skill

Integration reference for Plankton (credit: @alxfazio), a write-time code quality enforcement system for Claude Code. Plankton runs formatters and linters on every file edit via PostToolUse hooks, then spawns Claude subprocesses to fix violations the agent didn't catch.

## When to Use

- You want automatic formatting and linting on every file edit (not just at commit time)
- You need defense against agents modifying linter configs to pass instead of fixing code
- You want tiered model routing for fixes (Haiku for simple style, Sonnet for logic, Opus for types)
- You work with multiple languages (Python, TypeScript, Shell, YAML, JSON, TOML, Markdown, Dockerfile)

## How It Works

### Three-Phase Architecture

Every time Claude Code edits or writes a file, Plankton's `multi_linter.sh` PostToolUse hook runs:

```
Phase 1: Auto-Format (Silent)
鈹溾攢 Runs formatters (ruff format, biome, shfmt, taplo, markdownlint)
鈹溾攢 Fixes 40-50% of issues silently
鈹斺攢 No output to main agent

Phase 2: Collect Violations (JSON)
鈹溾攢 Runs linters and collects unfixable violations
鈹溾攢 Returns structured JSON: {line, column, code, message, linter}
鈹斺攢 Still no output to main agent

Phase 3: Delegate + Verify
鈹溾攢 Spawns claude -p subprocess with violations JSON
鈹溾攢 Routes to model tier based on violation complexity:
鈹?  鈹溾攢 Haiku: formatting, imports, style (E/W/F codes) 鈥?120s timeout
鈹?  鈹溾攢 Sonnet: complexity, refactoring (C901, PLR codes) 鈥?300s timeout
鈹?  鈹斺攢 Opus: type system, deep reasoning (unresolved-attribute) 鈥?600s timeout
鈹溾攢 Re-runs Phase 1+2 to verify fixes
鈹斺攢 Exit 0 if clean, Exit 2 if violations remain (reported to main agent)
```

### What the Main Agent Sees

| Scenario | Agent sees | Hook exit |
|----------|-----------|-----------|
| No violations | Nothing | 0 |
| All fixed by subprocess | Nothing | 0 |
| Violations remain after subprocess | `[hook] N violation(s) remain` | 2 |
| Advisory (duplicates, old tooling) | `[hook:advisory] ...` | 0 |

The main agent only sees issues the subprocess couldn't fix. Most quality problems are resolved transparently.

### Config Protection (Defense Against Rule-Gaming)

LLMs will modify `.ruff.toml` or `biome.json` to disable rules rather than fix code. Plankton blocks this with three layers:

1. **PreToolUse hook** 鈥?`protect_linter_configs.sh` blocks edits to all linter configs before they happen
2. **Stop hook** 鈥?`stop_config_guardian.sh` detects config changes via `git diff` at session end
3. **Protected files list** 鈥?`.ruff.toml`, `biome.json`, `.shellcheckrc`, `.yamllint`, `.hadolint.yaml`, and more

### Package Manager Enforcement

A PreToolUse hook on Bash blocks legacy package managers:
- `pip`, `pip3`, `poetry`, `pipenv` 鈫?Blocked (use `uv`)
- `npm`, `yarn`, `pnpm` 鈫?Blocked (use `bun`)
- Allowed exceptions: `npm audit`, `npm view`, `npm publish`

## Setup

### Quick Start

> **Note:** Plankton requires manual installation from its repository. Review the code before installing.

```bash
# Install core dependencies
brew install jaq ruff uv

# Install Python linters
uv sync --all-extras

# Start Claude Code 鈥?hooks activate automatically
claude
```

No install command, no plugin config. The hooks in `.claude/settings.json` are picked up automatically when you run Claude Code in the Plankton directory.

### Per-Project Integration

To use Plankton hooks in your own project:

1. Copy `.claude/hooks/` directory to your project
2. Copy `.claude/settings.json` hook configuration
3. Copy linter config files (`.ruff.toml`, `biome.json`, etc.)
4. Install the linters for your languages

### Language-Specific Dependencies

| Language | Required | Optional |
|----------|----------|----------|
| Python | `ruff`, `uv` | `ty` (types), `vulture` (dead code), `bandit` (security) |
| TypeScript/JS | `biome` | `oxlint`, `semgrep`, `knip` (dead exports) |
| Shell | `shellcheck`, `shfmt` | 鈥?|
| YAML | `yamllint` | 鈥?|
| Markdown | `markdownlint-cli2` | 鈥?|
| Dockerfile | `hadolint` (>= 2.12.0) | 鈥?|
| TOML | `taplo` | 鈥?|
| JSON | `jaq` | 鈥?|

## Pairing with ECC

### Complementary, Not Overlapping

| Concern | ECC | Plankton |
|---------|-----|----------|
| Code quality enforcement | PostToolUse hooks (Prettier, tsc) | PostToolUse hooks (20+ linters + subprocess fixes) |
| Security scanning | AgentShield, security-reviewer agent | Bandit (Python), Semgrep (TypeScript) |
| Config protection | 鈥?| PreToolUse blocks + Stop hook detection |
| Package manager | Detection + setup | Enforcement (blocks legacy PMs) |
| CI integration | 鈥?| Pre-commit hooks for git |
| Model routing | Manual (`/model opus`) | Automatic (violation complexity 鈫?tier) |

### Recommended Combination

1. Install ECC as your plugin (agents, skills, commands, rules)
2. Add Plankton hooks for write-time quality enforcement
3. Use AgentShield for security audits
4. Use ECC's verification-loop as a final gate before PRs

### Avoiding Hook Conflicts

If running both ECC and Plankton hooks:
- ECC's Prettier hook and Plankton's biome formatter may conflict on JS/TS files
- Resolution: disable ECC's Prettier PostToolUse hook when using Plankton (Plankton's biome is more comprehensive)
- Both can coexist on different file types (ECC handles what Plankton doesn't cover)

## Configuration Reference

Plankton's `.claude/hooks/config.json` controls all behavior:

```json
{
  "languages": {
    "python": true,
    "shell": true,
    "yaml": true,
    "json": true,
    "toml": true,
    "dockerfile": true,
    "markdown": true,
    "typescript": {
      "enabled": true,
      "js_runtime": "auto",
      "biome_nursery": "warn",
      "semgrep": true
    }
  },
  "phases": {
    "auto_format": true,
    "subprocess_delegation": true
  },
  "subprocess": {
    "tiers": {
      "haiku":  { "timeout": 120, "max_turns": 10 },
      "sonnet": { "timeout": 300, "max_turns": 10 },
      "opus":   { "timeout": 600, "max_turns": 15 }
    },
    "volume_threshold": 5
  }
}
```

**Key settings:**
- Disable languages you don't use to speed up hooks
- `volume_threshold` 鈥?violations > this count auto-escalate to a higher model tier
- `subprocess_delegation: false` 鈥?skip Phase 3 entirely (just report violations)

## Environment Overrides

| Variable | Purpose |
|----------|---------|
| `HOOK_SKIP_SUBPROCESS=1` | Skip Phase 3, report violations directly |
| `HOOK_SUBPROCESS_TIMEOUT=N` | Override tier timeout |
| `HOOK_DEBUG_MODEL=1` | Log model selection decisions |
| `HOOK_SKIP_PM=1` | Bypass package manager enforcement |

## References

- Plankton (credit: @alxfazio)
- Plankton REFERENCE.md 鈥?Full architecture documentation (credit: @alxfazio)
- Plankton SETUP.md 鈥?Detailed installation guide (credit: @alxfazio)

## ECC v1.8 Additions

### Copyable Hook Profile

Set strict quality behavior:

```bash
export ECC_HOOK_PROFILE=strict
export ECC_QUALITY_GATE_FIX=true
export ECC_QUALITY_GATE_STRICT=true
```

### Language Gate Table

- TypeScript/JavaScript: Biome preferred, Prettier fallback
- Python: Ruff format/check
- Go: gofmt

### Config Tamper Guard

During quality enforcement, flag changes to config files in same iteration:

- `biome.json`, `.eslintrc*`, `prettier.config*`, `tsconfig.json`, `pyproject.toml`

If config is changed to suppress violations, require explicit review before merge.

### CI Integration Pattern

Use the same commands in CI as local hooks:

1. run formatter checks
2. run lint/type checks
3. fail fast on strict mode
4. publish remediation summary

### Health Metrics

Track:
- edits flagged by gates
- average remediation time
- repeat violations by category
- merge blocks due to gate failures
