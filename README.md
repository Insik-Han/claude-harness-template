# Claude Harness Template

A project-agnostic [Claude Code](https://docs.anthropic.com/en/docs/claude-code) harness template — agents, rules, skills, and hooks ready to drop into any repository.

## What's Included

```
.
├── CLAUDE.md                          # Project map template (fill in {{PLACEHOLDERS}})
├── .mcp.json                          # MCP server config (empty, add your own)
└── .claude/
    ├── settings.json                  # Permissions + hooks
    ├── agents/
    │   ├── code-reviewer.md           # Architecture & code quality review
    │   ├── entropy-detector.md        # Doc staleness & dead code detection
    │   └── security-reviewer.md       # Auth/secrets/input validation audit
    ├── rules/
    │   ├── development-workflow.md    # Superpowers-based dev flow
    │   ├── git-workflow.md            # Conventional Commits + PR flow
    │   ├── security.md                # Secret management & parameterized queries
    │   └── testing.md                 # TDD requirement
    └── skills/
        ├── gotchas/SKILL.md           # Record session pitfalls to docs/gotchas.md
        ├── harness-audit/SKILL.md     # Audit harness config quality
        └── session-retro/SKILL.md     # End-of-session learning extraction
```

## Quick Start

1. **Copy into your project**

   ```bash
   # Clone and copy (remove .git to avoid nesting)
   git clone https://github.com/Insik-Han/claude-harness-template.git /tmp/harness
   cp -r /tmp/harness/.claude /tmp/harness/CLAUDE.md /tmp/harness/.mcp.json your-project/
   rm -rf /tmp/harness
   ```

2. **Fill in CLAUDE.md** — Replace `{{PLACEHOLDERS}}` with your project details:
   - `{{PROJECT_NAME}}` — Your project name
   - `{{PROJECT_DESCRIPTION}}` — One-line description
   - `{{FRAMEWORK}}` — Your framework (Next.js, Remix, etc.)
   - Runtime constraints, commands, tech stack

3. **Create supporting docs** (recommended):
   ```bash
   mkdir -p docs
   touch docs/ARCHITECTURE.md docs/GOLDEN_PRINCIPLES.md docs/gotchas.md
   ```

4. **Customize** — Add/remove agents, skills, and rules to fit your project

## Hooks

Pre-configured hooks in `.claude/settings.json`:

| Hook | Trigger | Purpose |
|------|---------|---------|
| `PreToolUse` | Bash commands | Blocks dangerous operations (`rm -rf`, `DROP TABLE`, `--force`, `--no-verify`) |
| `Stop` | Session ends | Warns about uncommitted doc/rule changes |

## Agents

| Agent | Model | When to Use |
|-------|-------|-------------|
| `code-reviewer` | sonnet | After implementing or refactoring code, before PR |
| `entropy-detector` | sonnet | Weekly, before releases, after major refactors |
| `security-reviewer` | sonnet | After auth/secrets/input validation changes |

## Skills

| Skill | Command | Purpose |
|-------|---------|---------|
| gotchas | `/gotchas` | Record pitfalls discovered during a session |
| harness-audit | `/harness-audit` | Audit harness configuration against best practices |
| session-retro | `/session-retro` | Extract and persist session learnings |

## Rules

| Rule | Enforces |
|------|----------|
| `development-workflow.md` | Brainstorm -> Plan -> Execute -> Review -> Verify -> PR |
| `git-workflow.md` | Conventional Commits, PR-based workflow |
| `testing.md` | TDD (RED -> GREEN -> REFACTOR), 80%+ coverage |
| `security.md` | Env-based secrets, parameterized queries, security-reviewer usage |

## Designed For

This template uses the [Superpowers](https://github.com/anthropics/superpowers) plugin workflow. The development workflow rule references Superpowers commands (`/superpowers:brainstorming`, `/superpowers:writing-plans`, etc.). If you don't use Superpowers, replace those references with your own workflow.

## License

MIT
