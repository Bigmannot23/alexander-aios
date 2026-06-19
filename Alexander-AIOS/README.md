# Alexander-AIOS

This is Alexander Minnick's ClaudeOps second brain: a single local monorepo of
documentation, structure, templates, and indexes. It is the **brain**, not a
product. It holds doctrine, research, review, and learning workflows — not
shippable application code.

## Start here

1. [`CLAUDE.md`](CLAUDE.md) — short router. Read this first, every session.
2. [`00_System/Master-Blueprint-V1.md`](00_System/Master-Blueprint-V1.md) — canonical doctrine (the warehouse).
3. [`OPERATING_DOCTRINE.md`](OPERATING_DOCTRINE.md) — the binding rules extracted from the blueprint.
4. [`CURRENT_STATE.md`](CURRENT_STATE.md) — what's actually built right now.

## What this repo is

A multi-system, repo-native operating stack: AIOS (this brain), ClaudeOps
(reusable AI operating machinery), a Productization Lab, EntryLens (pointer
only — engine code lives elsewhere), AlphaLab (bounded trading research),
a Course/YouTube learning refinery, research library, decision logs, proof
logs, and a dashboard checklist. See `00_System/Master-Blueprint-V1.md` §2
for the full system map.

## What this repo is not

- Not the EntryLens product repo (deterministic engine code lives in a
  separate `entrylens/` repo, created later).
- Not a live trading tool. No broker, account, order, or P&L surfaces.
- Not an automation platform. No hooks, routines, or Dispatch workflows
  exist yet — see `01_ClaudeOps/Hooks/README.md` and
  `01_ClaudeOps/Routines/README.md` for why.

## Folder map

| Folder | Purpose |
|---|---|
| `00_System/` | Operating constitution: doctrine, safety, source-of-truth, indexes |
| `01_ClaudeOps/` | AI operating machinery indexes (skills, subagents, scripts, permissions) |
| `02_Productization-Lab/_index.md` | Convert proven internal workflows into sellable infrastructure |
| `03_EntryLens/` | Pointer/staging only for the EntryLens product |
| `04_AlphaLab/` | Bounded trading research, review, and mistake-mining (markdown only) |
| `05_Course-Library/_index.md` | Permitted course material → transformed notes |
| `06_YouTube-Lesson-Library/` | YouTube learning refinery: transcript → lesson → promotion |
| `07_Research-Library/_index.md` | General research staging |
| `08_Decision-Logs/_index.md` | ADR-style decision records |
| `09_Proof-Logs/_index.md` | Evidence that a change actually works |
| `10_Dashboards/_index.md` | Maturity model + KPI checklist (not an app) |
| `11_Archive/_index.md` | Monthly-pruned cold storage |
| `Templates/` | Reusable templates for every workflow above |
| `.claude/` | Claude Code settings, rules, skills, and agents indexes |

Every `_index.md` (and `00_System` `-Index.md`) states what belongs, what
doesn't, the read-first file, and the source-of-truth files for that area.
