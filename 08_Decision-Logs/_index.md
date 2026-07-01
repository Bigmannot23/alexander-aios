---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md Appendix B
---

# Decision Logs Index

## What belongs here

Individual decision logs (ADRs), one file per real decision, written with
`Templates/decision-log.md`. Every entry here should also get a row in
`00_System/Decision-Index.md` (the repo-wide searchable ledger).

## What does not belong here

Day-to-day choices that don't change doctrine, architecture, or a system
boundary. Routine task notes. The ledger view itself (that's
`00_System/Decision-Index.md`) — this folder holds the full text.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` Appendix B (decision-log fields) and
`00_System/Decision-Index.md` (the cross-repo ledger this folder feeds).

---

## Status

Eleven decisions logged, each with a row in `00_System/Decision-Index.md`:
ADR-0001 (trader-intelligence-company, 2026-06-19), ADR-0002 (permission
posture, 2026-06-21), DIRECTION-0001 (market-context evidence layer — direction
captured, 2026-06-22), ADR-0003 (EntryLens verified-state record, 2026-06-22),
ADR-0004 (AlphaLab predicate-candidate register init, 2026-06-23), ADR-0005
(adopt FAST_EL_V1, 2026-06-24), ADR-0006 (EL-ADR-017 puts tieRule pinned + Lane B
puts arc shipped, 2026-06-24), ADR-0007 (float-replay lock closed, 2026-06-25),
ADR-0008 (EntryLens product identity ratified, 2026-06-26), ADR-0009 (EL-ADR-022
pullback/continuation ruling recorded, 2026-06-27), and ADR-0010 (EntryLens
engine state — calendar / five-states / EL-ADR-025 hardening, 2026-07-01).
