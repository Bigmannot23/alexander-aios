---
read_first: CLAUDE.md
source_of_truth: this file (it is the index of indexes)
---

# Retrieval Index

## What belongs here

A flat list of every index file in the repo, one line each, so a cold
session (or a person) can find the right place without grepping the whole
tree. This is the "index of indexes."

## What does not belong here

Content. This file only points; it never explains a topic in depth.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

This file is the source of truth for "where do I find X" — but keep it in
sync. If it drifts from the real folder structure, fix this file or flag it
in the next weekly-brain-audit.

---

## Index of indexes

| Index | Folder/topic |
|---|---|
| `00_System/Retrieval-Index.md` | This file |
| `00_System/Claim-Index.md` | Every audited claim, repo-wide |
| `00_System/Decision-Index.md` | Every decision log, repo-wide |
| `00_System/Proof-Index.md` | Every proof log, repo-wide |
| `01_ClaudeOps/Skills/_index.md` | Skills built and proposed |
| `01_ClaudeOps/Subagents/_index.md` | Subagents built and proposed |
| `01_ClaudeOps/Scripts/_index.md` | Manual scripts (pre-hook) |
| `01_ClaudeOps/Permissions/_index.md` | Permission rules and rationale |
| `02_Productization-Lab/_index.md` | Internal-workflow → product ideas |
| `03_EntryLens/Predicate-Candidates/_index.md` | EntryLens predicate staging |
| `04_AlphaLab/Education/_index.md` | AlphaLab education material |
| `04_AlphaLab/Setups/_index.md` | AlphaLab setup concepts (non-advisory) |
| `04_AlphaLab/No-Trade-Filters/_index.md` | AlphaLab avoid-condition candidates |
| `04_AlphaLab/Mistake-Taxonomy/_index.md` | AlphaLab repeated-mistake catalog |
| `04_AlphaLab/Replay-Drills/_index.md` | AlphaLab replay/practice drills |
| `04_AlphaLab/Review-Rubrics/_index.md` | AlphaLab session-review rubrics |
| `04_AlphaLab/Predicate-Candidates/_index.md` | AlphaLab-side predicate staging, pre-promotion |
| `05_Course-Library/_index.md` | Permitted course material |
| `06_YouTube-Lesson-Library/_index.md` | YouTube ingest pipeline |
| `07_Research-Library/_index.md` | General research staging |
| `08_Decision-Logs/_index.md` | Decision log storage (folder) |
| `09_Proof-Logs/_index.md` | Proof log storage (folder) |
| `10_Dashboards/_index.md` | Maturity model + KPI checklist |
| `11_Archive/_index.md` | Cold storage |
| `.claude/rules/_index.md` | Claude Code rules |
| `.claude/skills/_index.md` | Claude Code skills config |
| `.claude/agents/_index.md` | Claude Code subagents config |
