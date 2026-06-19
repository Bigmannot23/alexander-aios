---
read_first: CLAUDE.md
source_of_truth: OPERATING_DOCTRINE.md
---

# Claude Rules — Index

## What belongs here

Short, high-signal behavioral rules Claude should keep in mind on every
relevant task, independent of which skill or surface is in use. One file
per boundary or discipline.

## What does not belong here

- Doctrine reasoning and the "why" — that's `00_System/Master-Blueprint-V1.md`
  and `OPERATING_DOCTRINE.md`.
- Mechanical enforcement — that's `.claude/settings.json` (deny/ask/allow).
- Skill logic/procedure — that's `.claude/skills/**`.

## Load order assumption

Read `CLAUDE.md` first, then `OPERATING_DOCTRINE.md`, then this index, then
the rule file relevant to the task at hand. These files are not wired into
any automatic load/import mechanism yet — no hooks, no auto-context-
injection — consistent with the automation ladder (manual prompt before
script before hook). They're load-on-relevance reference rules until
proven.

## Files

| Rule | Scope | Purpose |
|---|---|---|
| `entrylens-trading-safety.md` | Unconditional | No trade advice, no Green authorization, ever |
| `alexander-aios-boundary.md` | Unconditional | Repo-as-truth, surface roles, automation ladder |
| `research-source-quality.md` | Unconditional | Classify sources before treating as fact |
| `repo-hygiene.md` | Unconditional | No raw data, small diffs, plan first |
| `decision-ledger-discipline.md` | Unconditional | Decisions are logged and superseded, not rewritten |
| `claude-surface-routing.md` | Unconditional | Which surface for which task, what's not in use yet |

All six are unconditional by design — this repo is still small and these
boundaries are global, not folder-specific. Path-scoped rules can be added
later if a rule only matters within one folder; don't force path-scoping
prematurely.
