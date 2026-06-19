---
last_updated: 2026-06-19
---

# Current State

Living snapshot. Update this whenever real state changes — stale
current-state is a named risk in the blueprint (§13).

## Where we are

**Block 1 of 7 (blueprint §9.1) — monorepo + `00_System/` skeleton +
short-router `CLAUDE.md` — just completed.** This is day-0 scaffold: structure,
templates, and indexes only. The first two skills, `route-task` and
`claim-audit`, have been authored; no loops have executed, no decisions
or proof entries are logged yet. A root `.gitignore` was added directly to `main` on
2026-06-19 (commit `a960b33`) and patched the same day after review to
close gaps (missing `tokens/`/`artifacts/raw/`/`live-account/`/
browser-profile patterns, non-functional raw-data anchoring).
`00_System/Safety-Policy.md` exists as the repo policy. The separate
enforcement file, `.claude/settings.json`, now exists (added 2026-06-19):
`defaultMode: "plan"`, bypass/auto modes disabled, a hard deny list over
secrets/credentials/tokens/broker/account/order/pnl/payment/banking/raw-data/
export-staging paths plus `mcp__*`, ask-before-Write/Edit and ask-before-git-
mutation, no hooks/MCP/routines/scripts/Dispatch. Not yet exercised against
a live session — see the maturity checklist below. The first behavioral
layer, `.claude/rules/**`, was also added 2026-06-19: six unconditional
rule files (EntryLens trading safety, Alexander-AIOS boundary, research
source quality, repo hygiene, decision-ledger discipline, Claude surface
routing) plus a README index — load-on-relevance reference rules, not yet
wired into any auto-load mechanism. The first decision log,
`08_Decision-Logs/ADR-0001-trader-intelligence-company.md` (status: accepted,
2026-06-19), was authored to record the doctrine that Claude becomes a
trader-intelligence company around EntryLens and does not trade — fixing the
intent of the ingestion system before any ingestion skill is built; it is
indexed in `00_System/Decision-Index.md`.

## Maturity (0–50 checklist — see `10_Dashboards/_index.md`)

- [x] Monorepo skeleton exists
- [x] `00_System/` doctrine + indexes exist
- [x] Short-router `CLAUDE.md` exists and fits one screen
- [x] Root `.gitignore` safety baseline verified and patched
- [ ] Claude Project created over the repo (manual UI step, not yet done)
- [ ] Templates exercised at least once
- [ ] First ~6 skills built and run (2/6: `route-task`, `claim-audit`)
- [ ] `.claude/settings.json` deny rule verified against a live test
- [ ] First `.claude/rules/` behavioral baseline verified against a live session
- [ ] First real ingest loop (3–5 sources, 1 promoted, 1 rejected)
- [x] First decision log entry (ADR-0001, accepted, 2026-06-19)
- [ ] First proof log entry
- [ ] First weekly-brain-audit run
- [ ] `entrylens/` separate product repo created
- [ ] First EntryLens predicate candidate (fixture + proof)
- [ ] AlphaLab review loop started (5+ reviews)
- [ ] Repo opened as an Obsidian vault

## What does not exist yet (by design)

- No hooks (`01_ClaudeOps/Hooks/README.md` — deferred until scripts pass
  5–10 clean manual runs)
- No routines (`01_ClaudeOps/Routines/README.md` — same gate)
- No Dispatch workflows
- No `entrylens/` product repo
- No subagents implemented yet. Two skills built (`route-task`,
  `claim-audit`; see `.claude/skills/_index.md`)
- No TradingView/browser connections

## Next manual steps (not automated)

1. Create a Claude Project over this repo with `CLAUDE.md`,
   `00_System/Safety-Policy.md`, `00_System/Source-of-Truth.md`, and
   `00_System/Master-Blueprint-V1.md` as curated knowledge.
2. Build the remaining ~4 of the first ~6 skills from blueprint §8.1
   (`route-task` and `claim-audit` are built).
