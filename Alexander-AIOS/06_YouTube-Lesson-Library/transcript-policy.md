---
read_first: this file, before anything else in this folder
source_of_truth: Master-Blueprint-V1.md §7.2
---

# Transcript Policy

## What belongs here

The rule for how raw YouTube transcripts are handled — separate from, and
stricter than, general source-quality or claim rules.

## What does not belong here

Source-quality scoring (`source-quality-rules.md`) or promotion mechanics
(`promotion-rules.md`).

## Read-first file

This file, first, before processing any transcript.

## Source-of-truth files

`Master-Blueprint-V1.md` §7.2 (rights/IP gate) and `00_System/
Rights-IP-Gate.md`.

---

## The rule

1. Raw transcripts land in `raw-transcripts/` only — gitignored, local
   machine only, never committed, never durable. Treat this folder as
   scratch space that could be wiped at any time.
2. Before any transformation, run the rights/IP gate
   (`00_System/Rights-IP-Gate.md`). Red verdict → delete the raw transcript,
   log the rejection in `06_YouTube-Lesson-Library/rejected/`, stop.
3. Green/amber verdict → produce a packet (`packets/`) that contains
   **transformed notes and metadata only** — never a verbatim transcript
   dump. Quote sparingly and only where fair-use-defensible.
4. The raw transcript itself is never the brain. If the packet/lesson is
   lost, re-deriving it from the raw transcript is acceptable; but the raw
   transcript is never treated as the durable record.
