---
id: proof-youtube-ingest-first-real-run
date: 2026-06-20
linked_decision: ADR-0001
---

# YouTube-Ingest — First Real Run (Saraev Claude Code course)

- **Date:** 2026-06-20
- **Workflow tested:** `06_YouTube-Lesson-Library/workflow.md` — full gate chain, first live transcript.
- **Scope:** one transcript, end to end. Raw transcript handled **in-session only**, never written to disk or committed.

This is the proof log called for by `workflow.md` §10 ("after the first real
transcript run, not before"). It records evidence that the workflow ran with
every gate honored and no raw material committed.

## Source identity (by reference only — no raw transcript stored)
- **Title:** Claude Code Full Course 4 Hours: Build & Sell (2026)
- **Creator:** Nick Saraev
- **Published:** 2026-02-12
- **Platform:** YouTube (video/course)
- **Processed:** ~2,294 lines / ~67k words, paraphrased in-session via a subagent that returned sanitized notes only.

## Change
First real packet + lesson produced by the youtube-ingest workflow, committed `f162191`:
- `06_YouTube-Lesson-Library/packets/2026-02-12-saraev-claude-code-full-course.md`
- `06_YouTube-Lesson-Library/lessons/2026-02-12-saraev-claude-code-full-course.md`

## Evidence — gate chain
- **Direct-request hard stop:** clear.
- **Rights/IP gate:** amber (restricted-use-by-default; "free to watch" ≠ a reuse license).
- **source-quality:** Accept with restrictions; rubric **16/20** (Credibility 3 · Evidence 3 · Recency 4 · Specificity 4 · Incentive hygiene 2).
- **claim-audit:** **Block until sourced.** Tallies — Supported: **0**; Unsupported: ~8 income/engagement/capability claims ($4M/yr, $300k+/mo, $10–15k/mo, 2,000+ students, "<10 min", "98–99%"); Needs-source/Outdated-risk: pricing/model/version/throughput claims. Safety-risk: **0**.
- **youtube-ingest:** Ready for human review.
- **trading-safety-review:** Pass (no findings; non-trading source).
- **product-scope-review:** In scope with constraints (hooks/MCP/agent-teams/dangerously-skip-permissions captured as Defer / HUMAN-REVIEW ONLY).

## Advice / hype correctly dropped (not adopted)
- Income/profit/value figures → unsupported marketing; the $4M/yr vs $300k+/mo figures flagged as mutually inconsistent.
- "any site in <10 min" / "98–99% accuracy" → unfalsifiable hype, dropped.
- dangerously-skip-permissions as a default working mode → rejected for this repo (manual permission posture).
- Broad agent credential/tool grants and auto-run flows → rejected (credential + blast-radius risk).

## Safety confirmations
- **No raw path committed.** `git status --porcelain` before commit showed only the two new files; no `raw-transcripts/` path; no `.txt` anywhere in the repo. Commit `f162191` contains exactly two files. The raw upload stayed outside the repo and was never copied in.
- **HUMAN-REVIEW ONLY labeling:** no EntryLens candidate lines were produced (non-trading source — "none found"). Automation/AIOS upgrade candidates were additionally prefixed `HUMAN-REVIEW ONLY` / `Defer` as a precaution.
- **Green:** not referenced in the source or the note (non-trading) → not reprinted, per the "where Green is referenced" rule. Not computed, authorized, or redefined.
- **No leakage:** nothing written to `03_EntryLens/`, `04_AlphaLab/`, `Claim-Index.md`, or `Decision-Index.md`. No promotion.

## Diff summary
2 files added (packet + lesson) on branch `claude/bold-pasteur-hzg1kt` (PR #22), commit `f162191`. This proof log + the `Proof-Index.md` ledger row are added in a follow-up commit.

## Result
**pass** (usable) — first real run of the workflow completed with all gates honored and no raw material committed.

## Known limitations
- Audited claims not yet logged to `00_System/Claim-Index.md` (human step, pending).
- Technique claims not yet verified against current `code.claude.com/docs` (claim-audit flags the need only).
- No promotion decision taken (`promotion-rules.md`), by design.
- PR #22 CI status / review comments not machine-verified this session — GitHub MCP tools were unavailable; reacting to webhook events instead.

## Recommended next proof
- After a docs-verification pass, re-run `claim-audit` on the technique claims to see whether any move from Needs-source to Supported, and log results to `Claim-Index.md`.
