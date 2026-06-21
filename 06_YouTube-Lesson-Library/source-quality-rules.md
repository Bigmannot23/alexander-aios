---
read_first: _index.md
source_of_truth: Master-Blueprint-V1.md §7.3
---

# Source Quality Rules (YouTube)

## What belongs here

How the general `00_System/Source-Quality-Rubric.md` (0–20 score) applies
specifically to YouTube sources — channel-level and video-level signals
worth checking.

## What does not belong here

The rubric itself (lives in `00_System/Source-Quality-Rubric.md`). This
file only adds YouTube-specific application notes.

## Read-first file

`_index.md`, then this file.

## Source-of-truth files

`00_System/Source-Quality-Rubric.md` (the rubric); this file applies it.

---

## YouTube-specific checks (feed into the five rubric categories)

- **Credibility**: does the channel show real, verifiable track record
  (not just "results" screenshots), or is the entire channel monetized
  around hype?
- **Evidence**: does the video show its work (chart replays with
  timestamps, not just claims), or is it all narration?
- **Recency**: is the content regime-dependent (a specific 2021 meme-stock
  environment) or mechanics-level and timeless?
- **Specificity**: are claims tied to a specific, observable, repeatable
  setup, or vague ("just trade with discipline")?
- **Incentive hygiene**: is the video selling a paid signal service, a
  course upsell, or a broker affiliate link? Heavy monetization pressure
  lowers this score even if the content sounds reasonable.

Score using `Templates/source-quality.md`, store the result in the
source's packet.

## AI-generated summaries — reliability cap

When the input is an AI-generated summary rather than a transcript or first-hand
viewing, the five checks above still apply to the **underlying video**, but the
summary itself is a secondary, machine-generated derivative:

- **Reliability is capped at medium — never high.** A summary can hallucinate or
  misattribute, so it can never reach the top credibility/evidence band on the
  strength of the summary alone, no matter how strong the underlying channel is.
- **Record the restriction "verify all claims against the primary source before
  any promotion."** The summary is not a citable source; a claim attributed only
  to it stays Unsupported / Needs-source through claim-audit.
- **Provenance is mandatory.** Capture which tool/model produced the summary in
  the packet front-matter (`summary_provenance:`) and set
  `input_trust: secondary-unverified` (see `lesson-template.md`).

This does not bypass the rights gate or claim-audit — all gates remain
independent and all must pass.
