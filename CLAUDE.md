# {{PROJECT_NAME}} — CLAUDE.md

> {{PROJECT_DESCRIPTION}}

## Knowledge Base

| Document                                           | Contents                      |
| -------------------------------------------------- | ----------------------------- |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)       | Layer structure & conventions |
| [docs/GOLDEN_PRINCIPLES.md](docs/GOLDEN_PRINCIPLES.md) | Mechanical rules          |
| [docs/gotchas.md](docs/gotchas.md)                 | Known pitfalls & workarounds  |

## Tech Stack

| Layer            | Technology                  |
| ---------------- | --------------------------- |
| Language         | {{LANGUAGE}}                |
| Framework        | {{FRAMEWORK}}               |
| Hosting          | {{HOSTING}}                 |
| Database         | {{DATABASE}}                |
| Auth             | {{AUTH}}                    |
| AI               | {{AI}}                      |
| UI               | {{UI}}                      |
| Testing          | {{TESTING}}                 |
| Linter/Formatter | {{LINTER}}                  |
| Package Manager  | {{PACKAGE_MANAGER}}         |
| Git Hooks        | {{GIT_HOOKS}}               |

<!-- Remove rows that don't apply to your project. Add rows as needed. -->

## Runtime Constraints

<!-- List constraints that agents MUST know (e.g., no Node.js APIs, CPU limits) -->

## Git Workflow

- **No direct push to main** — branch + PR
- Details: `.claude/rules/git-workflow.md` / `.claude/rules/development-workflow.md`

## Superpowers

| Command                                       | Purpose                 |
| --------------------------------------------- | ----------------------- |
| `/superpowers:brainstorming`                  | Requirements & design   |
| `/superpowers:writing-plans`                  | Implementation planning |
| `/superpowers:executing-plans`                | Plan execution (TDD)    |
| `/superpowers:test-driven-development`        | TDD cycle               |
| `/superpowers:systematic-debugging`           | Systematic debugging    |
| `/superpowers:verification-before-completion` | Pre-completion verify   |
| `/superpowers:requesting-code-review`         | Code review request     |

## Agents

| Agent               | Purpose                              | When to use             |
| ------------------- | ------------------------------------ | ----------------------- |
| `code-reviewer`     | Architecture & code quality check    | After src/ changes      |
| `security-reviewer` | Auth/secrets/input validation audit  | Security-related changes |
| `entropy-detector`  | Doc consistency & dead code detection | Periodic maintenance   |

## Skills

| Skill        | Command                           | Purpose                              |
| ------------ | --------------------------------- | ------------------------------------ |
| gotchas      | `/gotchas`                        | Record pitfalls to docs/gotchas.md   |
| harness-audit | `/harness-audit [--focus <area>]` | Audit harness configuration quality |
| session-retro | `/session-retro`                 | End-of-session learning extraction   |

## Testing

- TDD required — Details: `.claude/rules/testing.md`

<important if="auth|oauth|jwt|csp|secrets|security">

## Security

- Secrets via environment variables — never hardcode
- Use `security-reviewer` agent for security-related code changes

</important>

## Commands

```bash
pnpm dev              # Development server
pnpm check            # Lint + format + types
pnpm fix              # Auto-fix lint + format
pnpm verify           # check + test + build (required before PR)
pnpm test             # Run tests
```

## Hooks

- `PreToolUse (Bash)`: Block dangerous operations (rm -rf, DROP TABLE, --force)
- `SessionStart`: Show branch & change status
- `Stop`: Check for uncommitted doc changes
