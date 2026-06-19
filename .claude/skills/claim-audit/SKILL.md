---
name: claim-audit
description: Human-invoked claim auditor for Alexander-AIOS. Trigger when the user asks to "audit claims," "claim-audit this," "check this draft/note/summary for unsourced claims," or before any draft, source note, research summary, decision log, product claim, or EntryLens-related improvement enters source-of-truth. Given a pasted/attached draft, extracts every discrete statement and sorts it into facts, recommendations (the source's, not Claude's), assumptions, and open questions, then audits each factual/recommendation claim for source name, date, confidence, staleness risk, and trading/EntryLens-safety risk. Outputs an audit table and a final pass / pass with caveats / block until sourced / block for safety verdict. Never edits files by default — read-only analysis for human review.
---

# claim-audit

`claim-audit` is a **read-first, human-invoked classifier**. Given a
pasted draft, source note, research summary, decision-log draft, product
claim, or EntryLens-related improvement text, it extracts every discrete
claim, sorts it into one of four buckets, audits the factual and
recommendation buckets for sourcing/staleness/safety, and ends with one
of four verdicts. It never edits files by default, never performs
ingestion, never promotes a claim into `00_System/Claim-Index.md`, and
never computes or authorizes EntryLens Green.

## Source of truth — read only

This skill reads, but never edits during routine invocation:

- `.claude/rules/research-source-quality.md` — four-bucket rule (facts,
  recommendations, assumptions, open questions) and source-classification
  discipline
- `.claude/rules/entrylens-trading-safety.md` — trading-safety boundary
  and the verbatim Green definition
- `00_System/Master-Blueprint-V1.md` §7.4 — claim-audit requirement and
  raw-input → safe-brain-output transformation table
- `Templates/claim-audit.md` — the full audit form a human fills in if a
  claim is later promoted
- `00_System/Claim-Index.md` — the ledger a promoted claim is later
  logged to (by a human, not by this skill)
- `00_System/Source-Quality-Rubric.md` — source scoring categories this
  skill's confidence levels are consistent with

The blueprint wins on every conflict. This read-only rule governs the
skill's own auditing behavior — it is distinct from the one-time
authoring step that created this file and its index entries.

## Procedure

Run in order against the supplied text:

1. **Bucket pass** — sort every discrete statement into exactly one of:
   - Factual claims
   - Recommendations (the source's, never Claude's)
   - Assumptions
   - Open questions

   This enforces the four-bucket rule already required by
   `research-source-quality.md` rather than re-inventing it.

2. **Audit pass** — for every item in *Factual claims* and
   *Recommendations*, record source name, source date, confidence
   (low / medium / high / none given), sources against (any contradicting
   source found or named in the text, or "none found" — never left
   blank), and a use restriction (e.g. "research only," "requires
   independent verification," "rejected — prohibited"), per the full
   field set required by `Master-Blueprint-V1.md` §7.4 and
   `Templates/claim-audit.md`. Then assign exactly one status:
   - **Supported** — named source, date, confidence, sources-against
     check, and use restriction all present and consistent with the
     claim
   - **Unsupported** — no source given at all
   - **Needs-source** — partial sourcing (e.g. source named but undated,
     vague attribution like "people say," or sources-against/use
     restriction left unaddressed)
   - **Outdated-risk** — sourced but stale, or currency cannot be
     confirmed from the text
   - **Safety-risk** — see step 4; safety-risk always overrides any other
     status for that claim

   A claim with no source name/date present can never be marked
   `Supported`. A claim is never marked `Supported` while sources-against
   is blank or no use restriction is given — at best it is
   `Needs-source`.

3. **Stale scan** — for any claim touching a fast-moving fact (market
   structure/regime, pricing, product behavior, software/API behavior),
   flag `Outdated-risk` if the source is undated, the date is old
   relative to how fast that fact changes, or the text gives no way to
   confirm current accuracy. Note "requires current web verification" on
   that row. This skill flags the need only — it does not fetch the web
   or verify currency itself; that is a separate, explicit step.

4. **Safety scan** — run independently across *all four buckets*, not
   just facts. Flag any statement implying:
   - trade recommendations
   - buy/sell/call/put/entry/exit instructions
   - contracts, sizing, stops, or targets
   - probability, edge, win-rate, or expected-return claims
   - broker/account/order/P&L/payment/banking surfaces
   - AI-computed or AI-authorized EntryLens "Green"

   Any hit on the bucket pass’s output is reclassified `Safety-risk`,
   pulled into a separate "Safety-risk claims" block in the output, and
   if Green is referenced at all (even supportively), the skill reprints
   the verbatim definition from `entrylens-trading-safety.md`:

   > **Green = every required condition in the trader's locked declared
   > plan is deterministically satisfied right now. Green does not mean
   > buy, trade, enter, high probability, edge, or best time.**

## Output schema

```
## Claim Audit — <one-line description of input>

### Bucket separation
| # | Statement (as written) | Bucket |
|---|---|---|
| 1 | ... | Factual claim |
| 2 | ... | Recommendation (source's) |
| 3 | ... | Assumption |
| 4 | ... | Open question |

### Claim audit table
| # | Claim | Source name | Source date | Confidence | Sources against | Use restriction | Status | Note |
|---|---|---|---|---|---|---|---|---|
| 1 | ... | ... | ... | low/medium/high/none | ... or "none found" | research only / requires independent verification / rejected — prohibited | Supported/Unsupported/Needs-source/Outdated-risk/Safety-risk | ... |

### Safety-risk claims
(omit this section entirely if none found; otherwise list each with its
specific trigger category — e.g. "probability/edge claim," "AI-authorized
Green" — and reprint the verbatim Green definition if Green was
referenced at all)

### Verdict: Pass | Pass with caveats | Block until sourced | Block for safety
<one-line reason>
```

## Verdict precedence

Evaluate top to bottom; the first match wins:

1. **Block for safety** — any `Safety-risk` claim present, anywhere in
   the text.
2. **Block until sourced** — no safety-risk, but at least one
   load-bearing claim is `Unsupported` or `Needs-source`.
3. **Pass with caveats** — no safety-risk or unsourced blockers, but at
   least one claim is `Outdated-risk` or low-confidence.
4. **Pass** — every factual/recommendation claim is `Supported` with
   source, date, confidence, a sources-against check, and a use
   restriction; buckets are clean; nothing stale or unsafe.

## Test cases

1. **Clean sourced note** — every factual claim has a named source,
   date, stated confidence, a sources-against check ("none found" or a
   named contradicting source), and a use restriction; nothing
   trading-adjacent. Expect all rows `Supported` → verdict **Pass**.
2. **Missing sourcing** — a factual claim is stated with no source, date,
   sources-against check, or use restriction at all. Expect that row
   `Unsupported` → verdict **Block until sourced**.
3. **Stale claim, non-blocking** — a claim is dated but old relative to
   a fact that changes quickly, and nothing else in the text is unsourced
   or unsafe. Expect that row `Outdated-risk` → verdict **Pass with
   caveats**.
4. **Trading-adjacent language** — input contains "this setup wins 80%
   of the time, take the calls when X happens." Expect a `Safety-risk`
   row (probability/edge claim + buy/sell instruction) → verdict **Block
   for safety**.
5. **Bucket separation + Green misuse** — input mixes a source's own
   recommendation ("the course author recommends X"), an assumption, and
   an open question, correctly bucketed as Recommendation / Assumption /
   Open question (none miscast as a Factual claim), and also states
   EntryLens "Green" is already satisfied/authorized for a trade. Expect
   correct bucketing on the first three, a `Safety-risk` row on the Green
   statement with the verbatim Green definition reprinted → verdict
   **Block for safety**.

## What this skill must never do

- Edit any file by default — output is for human review only.
- Perform ingestion of any source, transcript, or course material.
- Promote a claim into `00_System/Claim-Index.md` — that stays a human
  step using `Templates/claim-audit.md`.
- Perform the web verification it flags as needed.
- Compute or authorize EntryLens Green.
- Soften a safety-risk claim into "educational" framing that still
  functions as advice.
