---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §8.2
---

# Subagents Index

## What belongs here

Every Claude Code subagent for this repo: built or proposed, one row each.
4 max to start, all read-only.

## What does not belong here

Skills (see `Skills/_index.md`) or scripts (see `Scripts/_index.md`).
Subagents are read-only reviewers/auditors, not task executors, at this
stage.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §8.2 (first subagents table) and §15
(do-not-build-yet: "Subagent swarm — 4 read-only specialists max to
start").

---

## Proposed first subagents (none built yet)

| # | Subagent | Invoke when | Output |
|---|---|---|---|
| 1 | product-doctrine-enforcer | any non-trivial EntryLens change | pass / block / unclear + which lock is at risk |
| 2 | trading-advice-boundary-auditor | any output with trading language | flagged phrases + verdict (reject on violation) |
| 3 | security-auditor | before commits; weekly | secret/credential/raw-asset findings |
| 4 | proof-reviewer | runtime-sensitive sprint | proof completeness verdict |

Each gets a one-page spec when built: mission, when/when-not, allowed/
denied files+tools, input/output format, pass/block/unclear criteria,
escalation, failure modes. None are built yet.
