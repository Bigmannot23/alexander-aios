---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md Appendix B, §1.4
---

# Proof Logs Index

## What belongs here

Individual proof logs, one file per verified change, written with
`Templates/proof-log.md`. Every entry here should also get a row in
`00_System/Proof-Index.md` (the repo-wide searchable ledger). Required for
any runtime-sensitive change per the proof lock (blueprint §1.4).

## What does not belong here

Unverified claims of completion — "no proof, no done" (blueprint §1.4).
The ledger view itself (that's `00_System/Proof-Index.md`) — this folder
holds the full evidence.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` Appendix B (proof-log fields) and §1.4 (proof
lock). `00_System/Proof-Index.md` (the cross-repo ledger this folder
feeds).

---

## Status

Proof logs entered 2026-06-20:

1. [`product-scope-review-first-live-runs.md`](product-scope-review-first-live-runs.md)
   — first read-only live-run sequence of the `product-scope-review` skill
   (2 clean artifacts passed, 1 adversarial artifact blocked; verdict: usable).
2. [`youtube-ingest-first-real-run.md`](youtube-ingest-first-real-run.md)
   — first real run of the `06_YouTube-Lesson-Library` ingest workflow on a
   live transcript (Saraev Claude Code course): full gate chain honored, no raw
   material committed; verdict: pass.

Both now have matching rows in `00_System/Proof-Index.md` (the earlier deferred
follow-up for entry 1 is resolved).
