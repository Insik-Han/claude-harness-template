---
name: harness-audit
description: "Audit Claude Code harness configuration (CLAUDE.md, settings.json, hooks, agents, skills, rules) against best practices. Use after adding/changing harness components, during periodic maintenance, or when you want to verify configuration quality. While entropy-detector checks code health, this skill inspects the harness configuration itself."
user-invocable: true
argument-hint: "[--focus area] — area: claude-md|hooks|agents|skills|rules|all"
---

# Harness Audit Skill

Audit Claude Code harness configuration (`.claude/` directory + `CLAUDE.md`) against best practices and provide improvement suggestions.

## Usage

```
/harness-audit                 # Audit all areas
/harness-audit --focus hooks   # Deep audit on hooks only
```

## Audit Steps

### 0. File Collection

Collect the following files via Read / Glob (skip if missing):

- `CLAUDE.md`
- `.claude/settings.json`
- `.claude/settings.local.json` (if exists)
- `.claude/agents/*.md`
- `.claude/skills/*/SKILL.md`
- `.claude/rules/*.md`

---

### 1. CLAUDE.md Quality

| Check | Criteria | Rationale |
|---|---|---|
| Line count | ~100 lines (under 200) | CLAUDE.md is a map, not an encyclopedia |
| Tech stack | Matches major package.json dependencies | Prevents agents from working with outdated assumptions |
| Commands | Matches package.json scripts | Prevents execution of non-existent commands |
| Skills table | Listed skills exist in `.claude/skills/` | Detect references to deleted skills |
| Agents table | Listed agents exist in `.claude/agents/` | Detect references to deleted agents |
| Hooks section | Listed hooks exist in `.claude/settings.json` | Check only major hooks for consistency |
| Link health | All listed paths actually exist | Broken links mislead agents |

---

### 2. settings.json — Hooks

| Check | Criteria | Rationale |
|---|---|---|
| PreToolUse matcher | Each hook has a `matcher` set | Without matcher, fires on all tools causing overload |
| Danger guard | Blocks `rm -rf`, `DROP TABLE`, `--force`, `--no-verify` | Prevent irreversible operations |

---

### 3. Agents

For each `.claude/agents/*.md`:

| Check | Criteria | Rationale |
|---|---|---|
| Required frontmatter | `name`, `description` exist | Required for auto-detection and invocation |
| Description quality | Includes WHEN (when to use), not just WHAT | Directly affects trigger accuracy |
| Model specification | Appropriate model explicitly set | Cost optimization |
| Reference paths | All referenced file paths actually exist | Broken references cause misses |
| Examples | Description includes concrete usage examples | Improves auto-invoke accuracy |

---

### 4. Skills

For each `.claude/skills/*/SKILL.md`:

| Check | Criteria | Rationale |
|---|---|---|
| Required frontmatter | `name`, `description`, `user-invocable` exist | Required for detection and menu display |
| Description quality | Includes both when to use and what it does | Prevents under-triggering |
| Line count | SKILL.md under 500 lines | Split to references/ if exceeded |

---

### 5. Rules

| Check | Criteria | Rationale |
|---|---|---|
| Coverage | testing, security, git-workflow exist at minimum | Basic quality gates |
| Cross-references | development-workflow.md references existing skills/agents | Workflow definition consistency |
| Duplication | Rules and CLAUDE.md don't duplicate the same content | Prevents update drift |

---

## Output Format

### Findings

| Severity | Criteria |
|---|---|
| **CRITICAL** | Directly impacts agent behavior (broken references, inconsistencies) |
| **HIGH** | Significant deviation from best practices |
| **MEDIUM** | Recommended improvement but no operational impact |
| **LOW** | Minor quality suggestions |

### Health Score

Rate each area on a 5-point scale.

### Recommendations

Up to 5 concrete actions in priority order.

---

## Guidelines

- Read actual files to verify — don't guess
- Report only substantive issues — not style preferences
- Include concrete fix for each finding
- If healthy, report as healthy — don't fabricate problems
