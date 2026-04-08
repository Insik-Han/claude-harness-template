---
name: session-retro
description: "Distill session learnings back into the repository before ending a session. Use at the end of sessions involving bug fixes, feature implementation, or debugging. Classifies learnings as gotcha (external pitfall) or harness (AI behavior improvement) and appends to appropriate harness components. Most effective when called just before commit/PR creation or as a completion signal."
user-invocable: true
---

# Session Retro

A triage skill that distills session learnings back into the repository.

## Why Persist to the Repository

Local memory only affects personal conversation context. Repository-persisted learnings benefit CI, other agents, and all future sessions.

## Classification Criteria

| Category | Definition | Example |
|----------|-----------|---------|
| **gotcha** | Non-intuitive behavior of external frameworks, libraries, or runtimes | "SQLite's CURRENT_TIMESTAMP is UTC" |
| **harness** | Insights that improve AI agent behavior | "Verify bugs on device before code fixes" |
| **skip** | Basic knowledge, temporary state, or things sufficiently captured in code | "React useState is independent" |

### Choosing Where to Save Harness Learnings

| Nature of Learning | Destination | Example |
|---|---|---|
| Always-applicable conventions | `.claude/rules/<topic>.md` | "Verify bugs on device" |
| Project-wide policy changes | `CLAUDE.md` | Adding new tools |
| Reusable workflows | `.claude/skills/` | Deploy procedures |
| Automatically enforced rules | hooks in `.claude/settings.json` | Format enforcement |

## Steps

1. **Read current state** — Review `docs/gotchas.md` and `.claude/rules/` existing entries

2. **Review the session** — Extract learning candidates from conversation context:
   - Errors encountered and their workarounds
   - Gaps between documentation and actual behavior
   - Correction instructions from the user
   - Non-obvious behaviors discovered during debugging

3. **Classify and present** — Classify candidates and ask user for confirmation:

   ```
   Organized session learnings:

   [gotcha]
   1. [Category] Description -> Workaround

   [harness]
   2. Description -> Destination: <path>

   [skip]
   3. Description (reason: basic knowledge)

   Select which to add.
   ```

4. **Duplicate check** — Verify no overlap with existing entries

5. **Append** — Add gotchas to `docs/gotchas.md`, harness learnings to selected destination

## Notes

- Never add entries without user confirmation
- Show reasoning for skip classifications (transparency for user override)
