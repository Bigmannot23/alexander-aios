---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md Appendix B
---

# Decision Index

## What belongs here

One row per decision log, repo-wide — every real decision that changed how
a system works, written with `Templates/decision-log.md` and stored in
`08_Decision-Logs/_index.md`'s folder. This is the searchable ledger; the
full ADRs live there.

## What does not belong here

The full decision text. This is a ledger, not the ADRs themselves.
Day-to-day choices that don't change doctrine or architecture don't need a
row — only decisions worth defending later.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` Appendix B (decision-log fields: id · date ·
decision · context · alternatives rejected · rationale · proof required ·
owner) and §3 (unresolved source-of-truth conflicts become a decision log).

---

## Ledger

No decisions have been logged yet. Add a row per decision using this
format:

| ID | Date | Decision | Owner | Proof required? | Link |
|---|---|---|---|---|---|
| ADR-0001 | 2026-06-19 | Claude becomes a trader-intelligence company around EntryLens (Claude does not trade) | bigmannot23 | no | [ADR-0001](../08_Decision-Logs/ADR-0001-trader-intelligence-company.md) |
| ADR-0002 | 2026-06-21 | Keep manual permission gating; reject dangerously-skip-permissions as a default working mode | bigmannot23 | no | [ADR-0002](../08_Decision-Logs/ADR-0002-permission-posture.md) |
| DIRECTION-0001 | 2026-06-22 | Market-Context Evidence Layer — two-evidence-type model (screen + trader-declared context predicates); DIRECTION CAPTURED, candidate, gated, not approved for build | bigmannot23 | no | [DIRECTION-0001](../08_Decision-Logs/DIRECTION-0001-market-context-evidence-layer.md) |
| ADR-0003 | 2026-06-22 | Informational state record — git-verified EntryLens session reconciliation; confirms evaluateLockedPlan slice + EL-ADR-007/008/009 DONE, EL-ADR-001–006 open (non-critical); decides nothing | bigmannot23 | no | [ADR-0003](../08_Decision-Logs/ADR-0003-entrylens-verified-state-2026-06-22.md) |
| ADR-0004 | 2026-06-23 | AlphaLab predicate-candidate register initialized from empty (PC-0001..PC-0005 RAW VWAP candidates, staged; PC-NNNN scheme fixed) | bigmannot23 | no | [ADR-0004](../08_Decision-Logs/ADR-0004-alphalab-predicate-candidate-register-init.md) |
| ADR-0005 | 2026-06-24 | Adopt FAST_EL_V1 — Lane B (single-pass build) default when meaning is pinned by repo truth or written approval; lanes A/B/C/D, STOP/state-check as interrupts, macro-tag prompt compression, three proof receipts, anti-process gate; hard carve-outs unchanged (Green ≠ buy, fail-closed, human-pinned doctrine/ADR/state) | bigmannot23 | no | [ADR-0005](../08_Decision-Logs/ADR-0005-adopt-fast-el-v1.md) |
| ADR-0006 | 2026-06-24 | Record: EL-ADR-017 pins puts tieRule (Set A + mirror, one-ADR consolidation resolving BD-0008 / BD-PC0001-PUTS); Lane B puts arc shipped — PC-0001/0002/0003 puts merged to EntryLens main (PR #33), engine 1085 tests/22 files green (cross-repo, transcribed; BD-0007) | bigmannot23 | no | [ADR-0006](../08_Decision-Logs/ADR-0006-puts-tierule-pinned-lane-b-arc-shipped.md) |
