---
read_first: transcript-policy.md
source_of_truth: Master-Blueprint-V1.md §7
---

# YouTube Lesson Library Index

## What belongs here

The YouTube learning refinery pipeline: `inbox/` (new links/transcripts to
process) → `raw-transcripts/` or `raw-summaries/` (gitignored, local-only) → `packets/`
(rights+quality+claim metadata) → `lessons/` (transformed reusable memory)
→ `promoted/candidates/` → `promoted/accepted/` or `promoted/rejected/` →
top-level `rejected/` for anything that never made it past packet stage. See
`workflow.md` for the step-by-step, one-transcript-at-a-time operating procedure
that sequences this pipeline with the `youtube-ingest` skill and the downstream
review gates.

## What does not belong here

Raw video files, raw transcript archives kept long-term (they're
gitignored local staging only, not durable storage), or any trading claim
adopted verbatim rather than transformed per
`00_System/Promotion-Rules.md`.

## Read-first file

`transcript-policy.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §7 (learning refinery), §7.5 (ingest output
schema), and the four policy files in this folder
(`transcript-policy.md`, `source-quality-rules.md`, `lesson-template.md`,
`promotion-rules.md`).

---

## Pipeline stages

| Stage | Folder | Gitignored? |
|---|---|---|
| 1. Inbox | `inbox/` | No |
| 2. Raw transcript | `raw-transcripts/` | **Yes** — local only |
| 2-alt. Raw AI summary (used instead of a transcript) | `raw-summaries/` | **Yes** — local only |
| 3. Packet | `packets/` | No |
| 4. Lesson | `lessons/` | No |
| 5a. Promotion candidate | `promoted/candidates/` | No |
| 5b. Accepted | `promoted/accepted/` | No |
| 5c. Rejected (post-candidate) | `promoted/rejected/` | No |
| Rejected (pre-candidate, e.g. failed rights/quality gate) | `rejected/` | No |

**AI-generated summaries** are a permitted **secondary, unverified** input that
can stand in for a raw transcript. On disk they are handled exactly like raw
transcripts (`raw-summaries/`, gitignored, never committed) and carry extra
trust constraints downstream — see `transcript-policy.md` and
`source-quality-rules.md`.

## Status

Empty pipeline — first real ingest loop (3–5 sources, promote 1, reject 1)
has not run yet. The Nate Herk AIOS transcript is the planned first test
case, per `Master-Blueprint-V1.md` front-matter source list (currently
missing/not yet supplied).
