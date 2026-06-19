---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §7.3
---

# Source Quality Rubric (0–20)

## What belongs here

The scoring rubric used to gate any source (course, YouTube video, article,
research doc) before it can be promoted toward a lesson or accepted delta.

## What does not belong here

Scores for actual sources — those live with the source's own packet/lesson
file (see `06_YouTube-Lesson-Library/` and `05_Course-Library/_index.md`),
using the `Templates/source-quality.md` template.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §7.3. This file expands that section into a usable
rubric; the blueprint wins on conflict.

---

## The rule

Score every source across five categories, 0–4 each, for a total of 0–20.
**A low score blocks promotion.** This is the filter against trading-guru
hype and low-quality course content.

| Category | 0 | 1–2 | 3–4 |
|---|---|---|---|
| **Credibility** | Anonymous / no track record | Some track record, unverified claims | Verifiable track record, transparent methodology |
| **Evidence** | Pure assertion, no data | Anecdotal or cherry-picked examples | Reproducible data, multiple examples, shown work |
| **Recency** | Stale, market regime has changed | Somewhat dated, partially applicable | Current, or timeless mechanics (not regime-dependent) |
| **Specificity** | Vague platitudes ("trade with discipline") | Generic rules without observable conditions | Concrete, observable, falsifiable conditions |
| **Incentive hygiene** | Selling a signal service / pump | Selling a course with inflated promises | No conflict of interest, or clearly disclosed |

## Score bands

- **0–8** — Reject. Do not promote past a packet. Log why in the rejected
  bucket.
- **9–14** — Research only. May inform a lesson, but claims need independent
  claim-audit before any promotion candidate is drafted.
- **15–20** — Eligible for promotion candidate, pending claim-audit and
  rights/IP gate (`Rights-IP-Gate.md`) passing independently. A high
  quality score does **not** bypass the rights gate or claim-audit — all
  three gates are independent and all must pass.

## Where this is used

`Templates/source-quality.md` is the working template. `Templates/
course-ingest.md` and `Templates/youtube-ingest.md` both require a score
from this rubric before a source can move past "packet" stage.
