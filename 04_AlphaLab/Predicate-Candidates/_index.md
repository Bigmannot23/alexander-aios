---
read_first: ../CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §1.4 (promotion lock)
---

# AlphaLab Predicate Candidates Index

## What belongs here

Deterministic predicate candidates *originating from AlphaLab research* —
the AlphaLab-side staging area, before anything is proposed across the
promotion lock into `03_EntryLens/Predicate-Candidates/_index.md`.

## What does not belong here

Anything wired into EntryLens. Anything non-deterministic. A candidate
moves to the EntryLens-side index only via a completed promotion packet
(`Templates/promotion.md`) and manual approval — see
`00_System/Promotion-Rules.md`. This index never edits that one directly.

## Read-first file

`04_AlphaLab/CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §1.4 (promotion lock) and
`00_System/Promotion-Rules.md`.

---

## Candidates

No predicate candidates staged yet. Add a row per candidate using this
format once any exist:

| ID | Date | Predicate (observable, falsifiable) | Source mistake/review | Promotion packet started? | Link |
|---|---|---|---|---|---|
| PC-0001 | 2026-06-23 | RAW: Price-to-VWAP position at bar close — close vs VWAP at that bar, with a declared tie rule. Must-declare: D1 config + tie rule. | generic public VWAP definition (research extraction) | No | |
| PC-0002 | 2026-06-23 | RAW: VWAP reclaim — a below->above close transition within a FINITE declared lookback, close-based. Must-declare: D1 config + finite lookback + close-based + N/M. N-of-M close-counting PINNED by EL-ADR-015 (entrylens-platform): **consecutive, anywhere-in-M, calls-only.** | generic public VWAP definition (research extraction) | No | EL-ADR-015 (entrylens-platform) |
| PC-0003 | 2026-06-23 | RAW: VWAP hold-above-N — last N consecutive closes above VWAP; reset on one close at/below. Must-declare: D1 config + N + fixed interval + explicit wick rule (default closes-only). | generic public VWAP definition (research extraction) | No | |
| PC-0004 | 2026-06-23 | RAW GUARD (not a setup predicate): Session-VWAP maturity guard — >= K bars or >= M minutes since anchor; withholds evaluation until met. Must-declare: D2 anchor/reset + K or M. | generic public VWAP definition (research extraction) | No | |
| PC-0005 | 2026-06-23 | RAW: VWAP sigma-band reach (touch or close-beyond at VWAP +/- k*sigma). Must-declare: k + side + condition type + timeframe. Sigma + line LOCKED by EL-ADR-010 (vol-weighted population std dev of TP about VWAP) and EL-ADR-011 (D1 line). HARD GATE before promotion: engine-level float-replay lock (accumulation order, precision, rounding) per replay-fixtures doctrine. | generic public VWAP definition (research extraction) | No | |
