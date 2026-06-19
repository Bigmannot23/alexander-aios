---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §1.4, §7.1, Appendix B
---

# Promotion Rules

## What belongs here

The rules for moving content up the learning-refinery hierarchy, and the
separate, stricter rule for moving anything from AlphaLab into EntryLens.

## What does not belong here

The promotion candidates themselves — those live in each system's own
`Predicate-Candidates/_index.md` (`03_EntryLens/`, `04_AlphaLab/`) or
`06_YouTube-Lesson-Library/promoted/`.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §1.4 (promotion lock), §7.1 (learning hierarchy),
Appendix B (promotion packet fields). This file summarizes those; the
blueprint wins on conflict.

---

## Learning-refinery hierarchy (do not collapse)

Video/course = raw material → transcript/source = private evidence →
lesson = reusable memory → promotion candidate = proposal → accepted
delta = system truth. **Raw material is never the brain.**

A source can only move one stage at a time, and only after the prior
stage's gates pass:

1. **Raw material → private evidence**: rights/IP gate must be green or
   amber with documented restriction (`Rights-IP-Gate.md`).
2. **Private evidence → lesson**: source-quality score recorded
   (`Source-Quality-Rubric.md`); transformed notes only, never a raw
   archive.
3. **Lesson → promotion candidate**: every factual/legal/financial/
   performance claim has a claim-audit entry (`Claim-Index.md`).
4. **Promotion candidate → accepted delta**: manual human approval, logged
   as a decision (`08_Decision-Logs/_index.md`).

## The EntryLens promotion lock (stricter — blueprint §1.4.5)

AlphaLab proposals enter EntryLens **only** via a manually approved
promotion packet + spec delta + tests. This is independent of, and in
addition to, the general learning-refinery hierarchy above. Never a direct
edit. Never an automatic promotion. See `Templates/promotion.md` for the
packet fields:

> source lesson/review · proposed deterministic predicate/reason-code ·
> avoid-condition definition · fixture idea · proof requirement · spec
> delta · manual-approval checklist.

Only deterministic artifacts cross into EntryLens — never recommendations,
probabilities, or contracts.

## Rejection is a first-class outcome

Every ingest loop should produce at least as many rejections as promotions
in the early stages (blueprint §9 build order step 6: "promote exactly one,
reject at least one"). A pipeline that never rejects anything is not
filtering.
