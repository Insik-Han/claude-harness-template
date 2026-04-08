# Development Workflow

Steps for new feature implementation:

0. **Research** — `gh search code` + `docs/` search for existing implementations. Prefer existing over custom
1. **Brainstorm** — `/superpowers:brainstorming` to explore requirements & design
2. **Plan** — `/superpowers:writing-plans` to create implementation plan
3. **Execute** — `/superpowers:executing-plans` to execute plan (TDD enforced by skill)
4. **Review** — `code-reviewer` agent for quality check, fix CRITICAL/HIGH issues
5. **Security** — `security-reviewer` agent when security-related code changes
6. **Verify** — `/superpowers:verification-before-completion` + `pnpm verify`
7. **PR** — `gh pr create` (link issues with `Closes #N`)

## Feedback Loop

When the agent struggles, treat it as a signal (Harness Engineering principle):

1. Identify missing tools, guardrails, or documentation
2. Update `docs/` or `docs/GOLDEN_PRINCIPLES.md`
3. Add new rules to architecture tests (prevent recurrence)

## Periodic Maintenance

- Run `entropy-detector` agent periodically to inspect codebase health
- Address detected inconsistencies as immediate fix PRs
