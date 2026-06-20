---
name: youtube-ingest
description: 'Human-invoked, video/course/podcast/transcript-specific research-ingest specialization for Alexander-AIOS. Trigger when the user asks to "ingest this video," "youtube-ingest this," "turn this video/course/podcast/transcript into a research note," or after a video/course/podcast/transcript source has cleared source-quality and the user wants a sanitized draft note. Distinct from Templates/youtube-ingest.md (the manual rights-gated, rubric-scored checklist that drives the 06_YouTube-Lesson-Library pipeline) — this skill is a separate, lighter advisory layer that never writes into that pipeline. Requires a prior source-quality verdict as input — stops if that verdict is Reject, Block for safety, or Human-review only, unless the user explicitly asks only for a short cautionary/rejection note recording why the source was not used. Given a vetted source, extracts and sorts content into a paraphrased source summary, timestamped concepts (paraphrased only, never transcript reproduction), supported claims (never marked supported without a claim-audit pass), unsupported/needs-source claims, useful patterns, risks/caveats, EntryLens-adjacent candidate material (always labeled HUMAN-REVIEW ONLY), ClaudeOS/AIOS upgrade candidates, decision-log candidates, rejected/no-use items, and open questions. Ends with one of six verdicts (ready for human review / needs source-quality first / needs claim-audit first / blocked by source-quality / blocked for trading safety / rejected — no useful action). Never stores raw transcript, copied course text, video/audio, clips, screenshots, or uploads. Never edits files by default, never writes to canonical doctrine or product files, never promotes candidate predicates/rules/replay-fixtures/hypotheses into implementation, never recommends trades/setups/entries/exits/contracts/sizing/stops/targets/probability/edge/win-rate/expected-return, and never computes or authorizes EntryLens Green.'
---

# youtube-ingest

**Read-first, human-invoked transformer** — the video/course/podcast/
transcript-specific specialization of `research-ingest`. Use it once a
video, course, podcast, or transcript source has cleared `source-quality`
and the user wants a sanitized, paraphrased draft research note. It is
stricter than `research-ingest` about timestamps, transcript handling, and
copyright, because video/course sources carry materially higher
reproduction risk than a generic article or paper.

**Not the same thing as `Templates/youtube-ingest.md`.** That template is
a manual, rights-gated, rubric-scored 10-step checklist that drives the
real `06_YouTube-Lesson-Library` pipeline (raw-transcripts → packets →
lessons → promotion). This skill is a separate, lighter, standalone
advisory layer: it produces a single human-review note and never writes
into that pipeline, never creates a packet or lesson file, and never
replaces the template's rights/rubric mechanics.

## Source of truth — read only

This skill reads, but never edits:

- `.claude/rules/research-source-quality.md`
- `.claude/rules/entrylens-trading-safety.md`
- `08_Decision-Logs/ADR-0001-trader-intelligence-company.md`
- `.claude/skills/source-quality/SKILL.md` (upstream gate — required input)
- `.claude/skills/claim-audit/SKILL.md` (required before any claim is
  marked supported)
- `.claude/skills/research-ingest/SKILL.md` (the general-purpose parent
  this skill specializes)
- `06_YouTube-Lesson-Library/transcript-policy.md` (raw-transcript
  handling rule this skill also honors)
- `Templates/youtube-ingest.md` (the separate manual checklist that drives
  the real Lesson Library pipeline — see disambiguation above)
- `00_System/Master-Blueprint-V1.md` §7 (ingest doctrine: raw material →
  private evidence → lesson → promotion candidate → system truth; never
  make raw material the brain)

## Procedure

Run in order:

1. **Direct-request hard stop.** Checked first, independent of the
   source's content or any gate below: if the user's own request — not
   something the source says — asks for a trade recommendation or signal,
   or asks Claude to authorize or compute EntryLens Green, stop
   immediately citing `.claude/rules/entrylens-trading-safety.md`. If the
   user's own request instead asks Claude to store the raw transcript or
   other raw source material, to reproduce a transcript beyond a brief
   fair-use-defensible snippet, or to download or fetch video/audio media,
   stop immediately citing `06_YouTube-Lesson-Library/transcript-
   policy.md` and the never-store-raw rule (step 14). Either way, do not
   continue to any step below.
2. **Require source-quality input.** If no source-quality verdict is
   supplied for this source, stop: **Needs source-quality first.**
3. **Gate on verdict.** If the verdict is Reject, Block for safety, or
   Human-review only, stop with **Blocked by source-quality** (or
   **Blocked for trading safety** if the verdict's underlying reason was a
   safety flag) — unless the user explicitly asks only for a short
   cautionary/rejection note recording why the source wasn't used. In that
   one case, produce only the header, gate inputs, and a single
   Rejected/no-use items entry, and still end in a blocked verdict.
4. **Record source identity** — title, creator/provider, URL/location if
   given (reference only, never re-published raw), published date if
   given, platform/type (video, course, podcast, or transcript).
5. **Determine rights/use restriction.** Treat video/course/podcast/
   transcript sources as **restricted-use-by-default** unless rights are
   clearly permissive (an explicit license, or the creator's own stated
   permission to reuse/reference). Record the determination and reason
   explicitly, and set the output's Citable scope field accordingly
   (paraphrase only, or brief fair-use quote permitted in the paraphrased
   summary). This determination never loosens the timestamped-concepts
   quoting limit in step 7 or the never-store-raw rule in step 14 — both
   apply regardless of rights.
6. **Paraphrased source summary pass.** Transform the source into prose in
   your own words. Never a transcript or quote dump.
7. **Timestamped concepts pass.** Record timestamp references only as
   paraphrased concept pointers (e.g., "~12:30 — creator explains a
   breakout-confirmation concept"), never as transcript reproduction.
   Quote sparingly and only where fair-use-defensible — a few words, never
   sentences or paragraphs — regardless of the step-5 determination.
8. **Claim extraction + claim-audit gate.** Extract every factual or
   performance claim. None may be marked "supported" without a claim-audit
   pass. If claim-audit hasn't run yet, is unavailable, or errors on a
   claim that would otherwise be marked supported, stop: **Needs
   claim-audit first.**
9. **Pattern + risk extraction.** Useful patterns, and risks/caveats.
10. **EntryLens-adjacent extraction.** Any predicate, rule, replay-fixture
    idea, or product hypothesis drawn from the source is extracted only
    under a `HUMAN-REVIEW ONLY:` prefix — staged, never wired in, never
    self-authorized.
11. **ClaudeOps/AIOS upgrade-candidate extraction.**
12. **Decision-log candidate extraction.**
13. **Rejected/no-use + open-questions pass.**
14. **Never write to canonical or pipeline locations; never store raw
    material.** Never write to `Master-Blueprint-V1.md`, `CLAUDE.md`,
    `OPERATING_DOCTRINE.md`, `.claude/rules/**`, `03_EntryLens/**`,
    `04_AlphaLab/**`, `00_System/Claim-Index.md`,
    `00_System/Decision-Index.md`, or any `06_YouTube-Lesson-Library/**`
    pipeline folder (`inbox/`, `raw-transcripts/`, `packets/`, `lessons/`,
    `promoted/**`) — record candidates in this skill's output only, never
    promote them. Never store raw transcript, copied course text,
    video/audio, clips, screenshots, or uploads, regardless of the step-5
    rights determination.
15. **Assign the final verdict** per the precedence below.

## Output schema

```markdown
## YouTube/video ingest header
<title, source type (video/course/podcast/transcript), creator/provider,
platform, date ingested>

## Gate inputs
- source-quality verdict: <verdict, date/source>
- claim-audit status: <Pass | Pass with caveats | Block until sourced | Block for safety | Not yet run>

## Source identity
- Title:
- Creator/provider:
- URL/location (reference only — never re-published raw):
- Published date (if given):
- Platform/type: video | course | podcast | transcript

## Rights/use restriction
- Determination: restricted-use-by-default | clearly-permissive
- Reason:
- Citable scope: paraphrase only | brief fair-use quote permitted

## Paraphrased source summary

## Timestamped concepts, paraphrased only
- ~MM:SS — <paraphrased concept pointer, never a transcript excerpt>

## Supported claims
<only claims that passed claim-audit>

## Unsupported / needs-source claims

## Useful patterns

## Risks and caveats

## EntryLens candidate material — HUMAN-REVIEW ONLY
<predicates / rules / replay-fixture ideas / product hypotheses, every
line prefixed HUMAN-REVIEW ONLY>

## ClaudeOS / AIOS upgrade candidates

## Decision-log candidates

## Rejected / no-use items

## Open questions

## Verdict
Ready for human review | Needs source-quality first | Needs claim-audit
first | Blocked by source-quality | Blocked for trading safety |
Rejected — no useful action
```

## Verdict precedence

First match wins:

1. **Blocked for trading safety** — source-quality said Block for safety,
   or the user's own request (step 1) asked for trade advice/a signal or
   EntryLens Green authorization.
2. **Blocked by source-quality** — verdict was Reject or Human-review
   only. The narrow exception in step 3 only narrows the output; it never
   changes this verdict.
3. **Needs source-quality first** — no verdict was supplied at all.
4. **Needs claim-audit first** — a claim needs claim-audit to be marked
   supported, and claim-audit hasn't run, is unavailable, or errored.
5. **Rejected — no useful action** — both gates cleared, but extraction
   produced nothing usable: every bucket is empty or "none found." Source
   identity and Rights/use restriction are always populated with at least
   a "not given" value and do not, by themselves, count as a non-empty
   bucket — at least one of the summary, timestamped concepts, supported
   claims, unsupported claims, useful patterns, EntryLens candidate
   material, upgrade candidates, or decision-log candidates sections must
   hold real content.
6. **Ready for human review** — none of the above; at least one bucket
   has real content.

## Test cases

1. **Clean success.** source-quality = Accept for claim-audit; the
   creator's own statement makes rights clearly-permissive; claim-audit
   already returned Pass; the source discusses a momentum-entry-timing
   concept. Expect: Rights determination = clearly-permissive; EntryLens
   candidate material populated, every line prefixed `HUMAN-REVIEW ONLY`;
   Verdict = Ready for human review.
2. **Verdict-input hard stops.** (a) No source-quality verdict is supplied
   at all → **Needs source-quality first.** (b) source-quality = Reject →
   output is the header, gate inputs, and a brief explanation only, with
   no content extraction → **Blocked by source-quality.**
3. **Block for safety, and the Human-review-only narrow exception.**
   (a) source-quality = Block for safety → **Blocked for trading safety.**
   (b) source-quality = Human-review only and the user asks for the full
   ingest treatment (not just a note) → the exception does not apply →
   **Blocked by source-quality.** (c) Same verdict, but the user asks only
   "write a short note on why we're not using this" → the narrow
   exception applies: output is the header, gate inputs, and one
   Rejected/no-use items entry → still **Blocked by source-quality.**
4. **Request-side hard stops, independent of source-quality.**
   (a) "Save the full transcript into the repo" → stop immediately at
   step 1, citing `transcript-policy.md`, nothing written, no extraction
   performed. (b) "Quote the whole transcript section here" → stop
   immediately at step 1, citing `transcript-policy.md`, refused beyond a
   fair-use-defensible snippet, no extraction performed. (c) "Download the
   video for me" → stop immediately at step 1, citing
   `transcript-policy.md`, refused, no extraction performed. (d) "Based on
   this, should I buy calls" or "does this make it Green" → stop
   immediately at step 1, citing `entrylens-trading-safety.md`, reprinting
   the Green definition verbatim if Green was mentioned, with no
   extraction performed.
5. **claim-audit unavailable mid-run.** source-quality = Accept for
   claim-audit; the source contains several factual/performance claims;
   claim-audit has not run and is not available this session → stop at
   step 8 → **Needs claim-audit first**; no claim is marked supported.

## What this skill must never do

- Perform actual ingestion, or write source notes or pipeline files, by
  default.
- Store raw transcript, copied course text, video/audio, clips,
  screenshots, or uploads, regardless of the rights/use determination.
- Reproduce a transcript verbatim beyond a brief, fair-use-defensible
  snippet.
- Download or fetch video/audio media.
- Write to canonical doctrine or product files, `Claim-Index.md`,
  `Decision-Index.md`, or any `06_YouTube-Lesson-Library/**` pipeline
  folder.
- Promote a candidate predicate, rule, replay fixture, or hypothesis into
  implementation.
- Mark a claim "supported" without a claim-audit pass.
- Recommend trades, setups, entries, exits, contracts, sizing, stops,
  targets, probability, edge, win-rate, or expected-return.
- Compute or authorize EntryLens Green. **Green = every required condition
  in the trader's locked declared plan is deterministically satisfied
  right now. Green does not mean buy, trade, enter, high probability,
  edge, or best time.**
