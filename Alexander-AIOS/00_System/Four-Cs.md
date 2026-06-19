---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §5
---

# Four Cs Architecture

## What belongs here

The Context → Connections → Capabilities → Cadence framework that governs
build order across every system in this repo. Reference this before adding
any tool, integration, or automation to any system.

## What does not belong here

System-specific implementation detail. This file holds the framework and
the per-system summary table only; deep detail lives in each system's own
brief (e.g. `04_AlphaLab/Product-Brief.md`).

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §5 (this file is a working copy of that section;
the blueprint wins on conflict).

---

## The rule

Build order is always **Context → Connections → Capabilities → Cadence.**
Do not add a tool unless it reduces repeated work, improves proof, reduces
risk, or increases retrieval speed.

- **Context** — what the system needs to know before it does anything
  (doctrine, current-state, indexes, specs, ADRs).
- **Connections** — what it's allowed to talk to (often: nothing, for a
  long time).
- **Capabilities** — what it can actually do (skills, scripts, engine code).
- **Cadence** — how often it runs and who reviews it.

## Per-system summary

| System | Context | Connections | Capabilities | Cadence |
|---|---|---|---|---|
| Alexander-AIOS | Doctrine, current-state, indexes | None | Route, skills, subagents | Daily capture, weekly audit, monthly prune, quarterly review |
| EntryLens OS | Doctrine, specs, ADRs | Local read-only `entrylens-state.json`; CI later | Engine, tests, fixtures, proof | Manual sprints → read-only audits later |
| AlphaLab OS | Research, memory, beliefs | Manual web research early | Notes, reviews, mistake mining, packets | Daily review → weekly synthesis |
| Live Vision Desk | Locked plan, no-trade rules, session log | `AlphaLab-TV` profile; optional TV webhook; read-only EntryLens bridge | Observe, warn, log, bookmark | Post-session first; live commentary very late |
| ClaudeOps | Reusable patterns from real systems | None | Template + skill compiling | Extracted after loops work |
| Lesson Library | Lessons, packets, candidates | You supply sources | Ingest → packet → lesson → candidate | Per-source audit; weekly promotion |
| Productization Lab | Product-ideas memory | None | Capture repeated workflow → MVP concept | Weekly memo |

Note the pattern: almost every system starts with **no connections**.
Connections are earned, not assumed.
