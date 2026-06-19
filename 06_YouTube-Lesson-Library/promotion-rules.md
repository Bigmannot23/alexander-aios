---
read_first: _index.md
source_of_truth: Master-Blueprint-V1.md §1.4, §7.1
---

# Promotion Rules (YouTube)

## What belongs here

How a YouTube lesson specifically moves through
`promoted/candidates/` → `promoted/accepted/` or `promoted/rejected/` —
the YouTube-pipeline application of the general
`00_System/Promotion-Rules.md`.

## What does not belong here

The general promotion rules and the EntryLens promotion lock (both live in
`00_System/Promotion-Rules.md`). This file only covers the YouTube
folder-routing mechanics.

## Read-first file

`_index.md`, then this file.

## Source-of-truth files

`00_System/Promotion-Rules.md` (general rule, wins on conflict).

---

## Mechanics

1. A lesson in `lessons/` that the operator judges strong enough to
   propose as a system delta gets copied (not moved) into
   `promoted/candidates/`, with every claim already audited
   (`00_System/Claim-Index.md`).
2. Manual review decides accept or reject.
3. Accepted → move the file to `promoted/accepted/`, log a decision
   (`08_Decision-Logs/_index.md`) if it changed how any system operates.
4. Rejected → move the file to `promoted/rejected/` with a one-line reason.
   Rejection is expected and healthy — see `00_System/Promotion-Rules.md`
   ("every ingest loop should produce at least as many rejections as
   promotions in the early stages").
5. If the lesson contains a deterministic predicate relevant to EntryLens,
   that's a *separate* track — see `03_EntryLens/Predicate-Candidates/_index.md`
   and the EntryLens promotion lock. Accepting a lesson here never by
   itself promotes anything into EntryLens.
