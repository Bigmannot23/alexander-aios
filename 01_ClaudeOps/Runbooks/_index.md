---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §8.1, §12
---

# Runbooks Index

## What belongs here

Manual standard operating procedures (SOPs) that have been proven by hand but
are not yet skills. A runbook is one rung below a skill on the automation
ladder: a written, repeatable procedure a human runs manually, step by step,
with no script, hook, or routine behind it. Filled outputs of a run live in
`runs/`.

## What does not belong here

Skills (tracked in `01_ClaudeOps/Skills/_index.md`, defined under
`.claude/skills/**`). Scripts, hooks, routines, Dispatch, or any automation —
none of those exist yet, by design (see `01_ClaudeOps/Hooks/README.md`,
`01_ClaudeOps/Routines/README.md`). Anything trading-, market-, or
account-related, automated or not.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §8.1 (skill promotion ladder) and §12 (automation
ladder). `OPERATING_DOCTRINE.md` §5 (surface routing). The `.claude/rules/`
files for the binding boundaries each runbook must respect.

---

## Where a runbook sits on the ladder

`manual prompt → runbook / manual-proof → skill (after 3–5 clean runs) →
script (if deterministic) → hook (after 5–10 clean runs) → routine`

A runbook is the "manual-proof" rung: the procedure is written down and run by
hand enough times to earn promotion to a skill. Never skip a rung.

## Runbooks

| # | Runbook | Purpose | Stage | Runs |
|---|---|---|---|---|
| 1 | [`lesson-decision-brief`](lesson-decision-brief.md) | one completed lesson packet → one safe decision/handoff (one best move or no-build) | runbook / manual-proof | `runs/` |

## The `runs/` folder

Each manual run writes one output file into `runs/`, named
`<source-slug>.brief.md`. The empty folder is held by `.gitkeep`; there is no
per-run `_index.md` — the run files themselves are the ledger, and any durable
outcome graduates into the relevant index (`00_System/Decision-Index.md`,
`00_System/Proof-Index.md`).

## Status

1 runbook (`lesson-decision-brief`), 1 acceptance run completed
([`runs/nate-herk-master-claude-code-beginner.brief.md`](runs/nate-herk-master-claude-code-beginner.brief.md)).
`lesson-decision-brief` graduates to a skill
(`.claude/skills/decision-brief/SKILL.md`) only after 3–5 clean manual runs
with zero safety/scope misfires.
