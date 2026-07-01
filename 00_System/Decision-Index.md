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

| ID | Date | Decision | Owner | Proof required? | Link |
|---|---|---|---|---|---|
| ADR-0001 | 2026-06-19 | Claude becomes a trader-intelligence company around EntryLens (Claude does not trade) | bigmannot23 | no | [ADR-0001](../08_Decision-Logs/ADR-0001-trader-intelligence-company.md) |
| ADR-0002 | 2026-06-21 | Keep manual permission gating; reject dangerously-skip-permissions as a default working mode | bigmannot23 | no | [ADR-0002](../08_Decision-Logs/ADR-0002-permission-posture.md) |
| DIRECTION-0001 | 2026-06-22 | Market-Context Evidence Layer — two-evidence-type model (screen + trader-declared context predicates); DIRECTION CAPTURED, candidate, gated, not approved for build | bigmannot23 | no | [DIRECTION-0001](../08_Decision-Logs/DIRECTION-0001-market-context-evidence-layer.md) |
| ADR-0003 | 2026-06-22 | Informational state record — git-verified EntryLens session reconciliation; confirms evaluateLockedPlan slice + EL-ADR-007/008/009 DONE, EL-ADR-001–006 open (non-critical); decides nothing | bigmannot23 | no | [ADR-0003](../08_Decision-Logs/ADR-0003-entrylens-verified-state-2026-06-22.md) |
| ADR-0004 | 2026-06-23 | AlphaLab predicate-candidate register initialized from empty (PC-0001..PC-0005 RAW VWAP candidates, staged; PC-NNNN scheme fixed) | bigmannot23 | no | [ADR-0004](../08_Decision-Logs/ADR-0004-alphalab-predicate-candidate-register-init.md) |
| ADR-0005 | 2026-06-24 | Adopt FAST_EL_V1 — Lane B (single-pass build) default when meaning is pinned by repo truth or written approval; lanes A/B/C/D, STOP/state-check as interrupts, macro-tag prompt compression, three proof receipts, anti-process gate; hard carve-outs unchanged (Green ≠ buy, fail-closed, human-pinned doctrine/ADR/state) | bigmannot23 | no | [ADR-0005](../08_Decision-Logs/ADR-0005-adopt-fast-el-v1.md) |
| ADR-0006 | 2026-06-24 | Record: EL-ADR-017 pins puts tieRule (Set A + mirror, one-ADR consolidation resolving BD-0008 / BD-PC0001-PUTS); Lane B puts arc shipped — PC-0001/0002/0003 puts merged to EntryLens main (PR #33), engine 1085 tests/22 files green (cross-repo, transcribed; BD-0007) | bigmannot23 | no | [ADR-0006](../08_Decision-Logs/ADR-0006-puts-tierule-pinned-lane-b-arc-shipped.md) |
| ADR-0007 | 2026-06-25 | Record: engine-level float-replay lock closed (BD-0002 RESOLVED) — EL-ADR-019 Accepted → PC-0005 sigma band + PP1–PP19 corpus shipped green (PR #41) → PC-0005 promotable flipped false→true across 6 spec mirrors (PR #42, 0135dc6); entrylens main 0e4ba03, git-verified (cross-repo, transcribed; BD-0007) | bigmannot23 | no | [ADR-0007](../08_Decision-Logs/ADR-0007-float-replay-lock-closed.md) |
| ADR-0008 | 2026-06-26 | Ratify EntryLens product identity — deterministic plan-/execution-alignment support tool for one human discretionary trader; NOT a scanner/signal generator/high-probability setup finder/market predictor/live AI trading assistant (Master-Blueprint §1.5, verbatim); canonical Green wording pinned (§1.1); doctrine ratification record, cites locked doctrine, amends nothing | bigmannot23 | no | [ADR-0008](../08_Decision-Logs/ADR-0008-entrylens-product-identity.md) |
| ADR-0009 | 2026-06-27 | Record: EL-ADR-022 (EntryLens) Accepted — VWAP_RECLAIM_PULLBACK_v1 pullback/continuation predicate gap (logged at EL-ADR-021 L77) resolved by ruling; adds close-only stateless PC-0006 (pullback-retest, τ default 0.002 convention-unverified) + PC-0007 (continuation-resumption, close above reclaim-leg high); three-leg merge PC-0002 + PC-0006 + PC-0007; anchoring = last-uninvalidated reclaim; stateless re-derivation preserves replay-lock; fail-closed; Green ≠ buy. RULING Accepted, predicates NOT yet built, D4 wiring deferred (cross-repo, transcribed; BD-0007) | bigmannot23 | no | [ADR-0009](../08_Decision-Logs/ADR-0009-el-adr-022-pullback-continuation-ruling.md) |
| ADR-0010 | 2026-07-01 | Record: EntryLens engine state — NYSE session calendar extended contiguous 2022–2028; five-color status bundle (GRAY/YELLOW/GREEN/ORANGE/RED) proven end-to-end + golden-locked (CORPUS=23), five-states milestone DONE; EL-ADR-025 engine-hardening MERGED (absent-wholeness→GRAY at GREEN boundary; opening-edge→GRAY), clearing EL-ADR-024 P2 precondition; f16 fixture-covered-only by design (not a gap). r01 blocked-by-design + producer P3 licensing tracked in BD-0011 (cross-repo, transcribed; BD-0007) | bigmannot23 | no | [ADR-0010](../08_Decision-Logs/ADR-0010-entrylens-engine-state-calendar-five-states-hardening.md) |
