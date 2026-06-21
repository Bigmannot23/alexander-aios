---
read_first: this file, before anything else in this folder
source_of_truth: Master-Blueprint-V1.md §7.2
---

# Transcript Policy

## What belongs here

The rule for how raw YouTube **inputs** are handled — raw transcripts and
AI-generated summaries alike — separate from, and stricter than, general
source-quality or claim rules.

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

## AI-generated summaries as input

An AI-generated summary of a video (produced by a third-party tool or model)
may be used as an input *instead of* a raw transcript. It is handled as **raw,
secondary, unverified** input — never as a finished note. The rule above
applies, with these additions:

1. The summary is **not the durable record and not a citable source.** It is a
   machine derivative that can hallucinate, misattribute, or omit. Default to
   processing it **in-session only**; if it must touch disk it goes **only** in
   `raw-summaries/` — gitignored, local-machine-only scratch, never committed,
   exactly like `raw-transcripts/`.
2. It still clears the **rights/IP gate**: an AI summary of a copyrighted video
   is a derivative of that video. Red verdict → delete, log to `rejected/`,
   stop.
3. If the summary embeds **verbatim transcript chunks** beyond a brief
   fair-use-defensible quote, those chunks fall under the raw-transcript
   reproduction rule — paraphrase them out; never carry them into a
   packet/lesson.
4. The resulting packet/lesson records provenance and lower trust in
   front-matter (`input_type: ai-summary`, `summary_provenance:`,
   `input_trust: secondary-unverified` — see `lesson-template.md`). Every
   factual claim drawn from a summary is **unverified until checked against the
   primary source** (see `source-quality-rules.md` and the claim-audit gate);
   nothing is marked "supported" on the summary's authority.
