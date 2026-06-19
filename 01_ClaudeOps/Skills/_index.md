---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §8.1
---

# Skills Index

## What belongs here

Every Claude Code skill for this repo: built or proposed, one row each,
with its promotion stage.

## What does not belong here

Skill implementation files themselves (those live wherever Claude Code
skills are actually defined for this repo, once built). Subagents (see
`Subagents/_index.md`) and scripts (see `Scripts/_index.md`) are tracked
separately.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §8.1 (first skills table) and §9 build order step
4 ("First skills (~10–12)").

---

## Promotion ladder

`manual prompt → used 3× → checklist → skill → script (if deterministic) →
hook (if enforcement) → routine (if scheduled, read-only only)`

## First skills (2 of 12 built)

| # | Skill | System | Trigger | Output | Class | Safety class | Status |
|---|---|---|---|---|---|---|---|
| 1 | route-task | AIOS | "where does this go?" | system/folder/skill | Immediate | n/a | Built — `.claude/skills/route-task/SKILL.md` |
| 2 | youtube-ingest | Lesson Library | new transcript | packet/lesson/claim-audit/candidate | Immediate | Research only | Proposed |
| 3 | course-ingest | Course Library | new permitted course material | transformed notes | Immediate | Research only | Proposed |
| 4 | claim-audit | AIOS | a factual/perf/legal claim | audited claim | V1 core | Research only | Built — `.claude/skills/claim-audit/SKILL.md` |
| 5 | source-quality-score | AIOS | new source | 0–20 score + gate | V1 core | Research only | Proposed |
| 6 | lesson-promote | AIOS | strong lesson | system delta candidate | V1 core | n/a | Proposed |
| 7 | decision-log | AIOS/all | a real decision | ADR entry | V1 core | n/a | Proposed |
| 8 | proof-log | AIOS/all | a verified change | proof entry | V1 core | n/a | Proposed |
| 9 | entrylens-predicate-candidate | EntryLens | lesson → rule | deterministic, non-advisory predicate | V1 core | EntryLens promotion candidate | Proposed |
| 10 | trading-review | AlphaLab | after session | non-advisory review | V1 core | Post-session only | Proposed |
| 11 | grill-me-trading-rules | AlphaLab | declaring a plan | gaps in invalidation/confirmation/no-trade | Immediate | Safe live cognitive support | Proposed |
| 12 | weekly-brain-audit | AIOS | weekly | decay/stale/missing-proof report | V1 core | n/a | Proposed |

Build ~10–12 of these, not 40. 2 of 12 built (route-task, claim-audit) —
this repo is at Block 1 of 7 (see `CURRENT_STATE.md`).
