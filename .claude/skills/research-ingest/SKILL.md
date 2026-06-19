---
name: research-ingest
description: Human-invoked research note builder for Alexander-AIOS. Trigger when the user asks to "ingest this source," "turn this into a research note," "research-ingest this," or after a source has already cleared source-quality and the user wants a sanitized draft note. Requires a prior source-quality verdict as input — stops if that verdict is Reject, Block for safety, or Human-review only, unless the user explicitly asks only for a short cautionary/rejection note recording why the source was not used. Given a vetted source, extracts and sorts content into source summary, supported claims (never marked supported without a claim-audit pass), unsupported/needs-source claims, useful patterns, risks/caveats, EntryLens-adjacent candidate material (predicates, replay-fixture ideas, product hypotheses — always labeled HUMAN-REVIEW ONLY), ClaudeOS/AIOS upgrade candidates, decision-log candidates, rejected/no-use items, and open questions. Ends with one of six verdicts (ready for human review / needs source-quality first / needs claim-audit first / blocked by source-quality / blocked for trading safety / rejected — no useful action). Never edits files by default, never writes directly to canonical doctrine or product files, never stores raw transcripts/screenshots/clips/uploads/audio/video/copyrighted raw material, never promotes candidate predicates/rules/replay-fixtures/hypotheses into implementation, never recommends trades/setups/entries/exits/contracts/sizing/stops/targets/probability/edge/win-rate/expected-return, and never computes or authorizes EntryLens Green.
---

# research-ingest

`research-ingest` is a **read-first, human-invoked transformer**. Given a
source that has already cleared `source-quality`, it extracts content into
labeled buckets, runs `claim-audit` against every candidate claim before
marking it supported, sanitizes everything EntryLens-adjacent as
HUMAN-REVIEW ONLY, and produces a draft research note for human review. It
never stores raw transcript/screenshot/clip/upload/audio/video/copyrighted
material, never writes to canonical doctrine or product files, never
promotes a candidate predicate/rule/replay-fixture/hypothesis into
implementation, and never computes or authorizes EntryLens Green.

## Source of truth — read only

This skill reads, but never edits during routine invocation:

- `.claude/rules/research-source-quality.md` — four-bucket discipline
  (facts, recommendations, assumptions, open questions), raw-storage ban,
  rights/IP-gate requirement before any promotion
- `.claude/rules/entrylens-trading-safety.md` — trading-safety boundary
  and the verbatim Green definition
- `08_Decision-Logs/ADR-0001-trader-intelligence-company.md` — names
  candidate predicates, candidate replay-fixture ideas, and product
  hypotheses as permitted, human-review-only output; Claude never acts,
  ingests, or self-authorizes on its own
- `.claude/skills/source-quality/SKILL.md` — the upstream gate; this skill
  never runs on a source that hasn't already produced a source-quality
  verdict
- `.claude/skills/claim-audit/SKILL.md` — invoked during this skill's own
  procedure (step 4 below) on every candidate factual or recommendation
  claim before it can be marked "supported" in this skill's output
- `00_System/Master-Blueprint-V1.md` §7 — raw-input → safe-brain-output
  transformation table this skill's bucket extraction follows

The blueprint wins on every conflict. This read-only rule governs the
skill's own ingestion behavior — it is distinct from the one-time
authoring step that created this file and its index entries.

## Procedure

Run in order against the supplied source and its prior source-quality
output:

1. **Require source-quality input.** This skill never runs on a bare
   source. If no `source-quality` verdict is supplied alongside the
   source, stop immediately with verdict **Needs source-quality first** —
   do not attempt to classify the source yourself.

2. **Gate on the source-quality verdict.**
   - **Reject** → stop with verdict **Blocked by source-quality**.
   - **Block for safety** → stop with verdict **Blocked for trading
     safety**.
   - **Human-review only** → stop with verdict **Blocked by
     source-quality**.
   - **Narrow exception** (applies to all three blocks above): if the
     user explicitly asks only for a short cautionary/rejection note
     recording *why* the source was not used (not a content extraction),
     produce only the Research ingest header, Gate inputs, and a
     one-paragraph Rejected/no-use items entry stating the source-quality
     verdict and reason, then end with the same blocked verdict. Never
     extract Source summary, Useful patterns, or any EntryLens-adjacent
     bucket in this exception path — the note documents the block, it
     does not lift it.
   - **Accept with restrictions** or **Accept for claim-audit** → proceed
     to step 3. Carry forward the use-permission field (e.g. "paraphrase
     only") — it constrains step 3's summary but does not by itself block
     proceeding.

3. **Source summary pass.** Write a short, paraphrased summary of the
   source's content and structure. Never quote more than a short, clearly
   attributed fragment; never reproduce the source's text at length
   regardless of use permission. This is a transformed summary, not an
   extract.

4. **Claim extraction + claim-audit gate.** Pull every discrete factual or
   recommendation-shaped statement out of the source, then run
   `claim-audit` on each claim during this invocation before assigning it
   to Supported vs Unsupported. A claim is placed in **Supported claims**
   only if `claim-audit` returns status `Supported` for it (named source,
   date, confidence, sources-against check, and use restriction all
   present and consistent). Any claim `claim-audit` marks `Unsupported`,
   `Needs-source`, or `Outdated-risk` goes into
   **Unsupported/needs-source claims** instead — never into Supported. If
   claims are present but claim-audit cannot be run in this invocation,
   stop with verdict **Needs claim-audit first** rather than guessing a
   claim into "supported."

5. **Pattern + risk extraction.** Separately from claims: pull recurring,
   non-claim structural/technical patterns into **Useful patterns** (e.g.
   "the source structures examples in three stages" — a pattern, not a
   factual or trading claim). Pull caveats, limitations, contradictions,
   or staleness risk into **Risks and caveats**.

6. **EntryLens-adjacent extraction — always HUMAN-REVIEW ONLY.** Anything
   that could become a candidate predicate, candidate rule, candidate
   replay-fixture idea, or product hypothesis must be extracted into
   **EntryLens candidate material (HUMAN-REVIEW ONLY)** and prefixed
   `HUMAN-REVIEW ONLY:` on every line item, per ADR-0001's "staged
   proposals only, not wired in, never self-authorized" boundary. This
   bucket never contains a trade recommendation, entry/exit instruction,
   contract/sizing/stop/target, or probability/edge/win-rate/expected-
   return claim — if the source's language would require one of those to
   state the candidate, drop the item into **Rejected/no-use items**
   instead with a one-line reason, rather than laundering it through
   "candidate" framing.

7. **ClaudeOps/AIOS upgrade extraction.** Anything suggesting an
   improvement to ClaudeOps machinery itself (a new skill idea, a process
   gap, a rule update worth proposing) goes into **ClaudeOS / AIOS upgrade
   candidates** — proposal only, never self-applied.

8. **Decision-log candidate extraction.** Anything that looks like it
   should become a future ADR (a real strategic choice implied by the
   source) goes into **Decision-log candidates** — naming the choice, not
   drafting the ADR itself.

9. **Rejected/no-use + open-questions pass.** Anything that doesn't earn a
   place in any bucket above (irrelevant, redundant, too speculative, or
   dropped per step 6's safety rule) goes into **Rejected/no-use items**
   with a one-line reason. Anything genuinely unresolved by the source
   goes into **Open questions**.

10. **Never write to canonical files.** This skill's entire output is a
    draft for human review. It never writes to
    `00_System/Master-Blueprint-V1.md`, `CLAUDE.md`,
    `OPERATING_DOCTRINE.md`, any `.claude/rules/**` file, any
    `03_EntryLens/**` or `04_AlphaLab/**` file, `00_System/Claim-Index.md`,
    or `00_System/Decision-Index.md`. If a human later wants to promote
    output from this skill, that promotion happens manually, outside this
    skill — the same way `claim-audit`'s output is promoted manually via
    `Templates/claim-audit.md`.

11. **Never store raw material.** This skill never writes the source's
    raw transcript, screenshot, clip, upload, audio, video, or other
    copyrighted raw material into any file, anywhere in the repo,
    regardless of the source-quality use-permission field. Only
    transformed, paraphrased, sanitized notes are ever produced.

12. **Assign verdict** — via the precedence table below.

## Output schema

```
## Research Ingest — <one-line description of source>

### Research ingest header
- Source title:
- Source-quality verdict carried forward:
- Date ingested (today's date, not source date):

### Gate inputs
- Source-quality verdict: Accept for claim-audit | Accept with restrictions | Human-review only | Reject | Block for safety
- Source-quality use permission: cite/use | paraphrase only | human-review only | do not ingest | reject
- Claim-audit available this run: yes | no

### Source summary
<short paraphrased summary — never a long verbatim extract>

### Supported claims
| # | Claim (paraphrased) | claim-audit status | Source name | Source date |
|---|---|---|---|---|
(only rows claim-audit would mark Supported — never populated without a
claim-audit pass)

### Unsupported / needs-source claims
| # | Claim (paraphrased) | claim-audit status | Why not supported |
|---|---|---|---|

### Useful patterns
- ...

### Risks and caveats
- ...

### EntryLens candidate material (HUMAN-REVIEW ONLY)
- HUMAN-REVIEW ONLY: candidate predicate — ...
- HUMAN-REVIEW ONLY: candidate replay-fixture idea — ...
- HUMAN-REVIEW ONLY: product hypothesis — ...
(omit any of the three sub-types with nothing to report; never omit the
HUMAN-REVIEW ONLY prefix on any line that remains)

### ClaudeOS / AIOS upgrade candidates
- ...

### Decision-log candidates
- ...

### Rejected / no-use items
- ... — <one-line reason>

### Open questions
- ...

### Verdict: ready for human review | needs source-quality first | needs claim-audit first | blocked by source-quality | blocked for trading safety | rejected — no useful action
<one-line reason>
```

## Verdict precedence

Evaluate top to bottom; the first match wins:

1. **Needs source-quality first** — no source-quality verdict was
   supplied alongside the source at all.
2. **Blocked for trading safety** — the supplied source-quality verdict
   is **Block for safety** (no narrow-exception cautionary note
   requested).
3. **Blocked by source-quality** — the supplied source-quality verdict is
   **Reject** or **Human-review only** (no narrow-exception cautionary
   note requested).
4. **Needs claim-audit first** — source-quality verdict allows
   progression (Accept with restrictions / Accept for claim-audit), but
   the source contains factual/recommendation claims and claim-audit
   could not be run in this invocation — so no claim can be marked
   Supported yet.
5. **Rejected — no useful action** — source-quality verdict allows
   progression and claim-audit ran where claims were present, but after
   extraction every bucket is empty or only Rejected/no-use items were
   produced (nothing worth a human reviewing).
6. **Ready for human review** — source-quality verdict allows
   progression, claim-audit ran against any claims present, every
   EntryLens-adjacent item is labeled HUMAN-REVIEW ONLY, and at least one
   non-empty, non-rejected bucket exists.

The narrow-exception cautionary note (Procedure step 2) still ends in
**Blocked by source-quality** or **Blocked for trading safety** — it
documents the block, it does not lift it.

## Test cases

1. **Clean accepted source, fully audited** — a named-author expert
   article, source-quality verdict **Accept for claim-audit** (reliability
   high, use permission cite/use, no safety flags), every factual claim
   already run through claim-audit and marked Supported, one useful
   structural pattern noted, no EntryLens-adjacent content. Expect:
   Supported claims populated, Useful patterns populated, EntryLens
   candidate material section empty/omitted → verdict **Ready for human
   review**.

2. **Source-quality verdict is Reject** — a vendor signal-selling video
   already run through source-quality with verdict **Reject** (evidence
   failure + incentive-hygiene failure together). User asks to
   "research-ingest this." Expect: no content extraction performed at all
   → verdict **Blocked by source-quality**, one-line reason citing the
   Reject verdict.

3. **Human-review-only source, explicit cautionary-note override** — a
   stale, undated blog post with source-quality verdict **Human-review
   only**. User explicitly says "I know it's human-review only, just give
   me a short note on why we're not using it." Expect: only the Research
   ingest header, Gate inputs, and a populated Rejected/no-use items
   section stating the source-quality verdict and the staleness/
   unverifiable-claims reason — no Source summary, no Useful patterns, no
   other bucket populated → verdict **Blocked by source-quality** (the
   override produces a note about the block, it does not unblock the
   source).

4. **Unsupported trading-adjacent claim requiring HUMAN-REVIEW labeling** —
   a source-quality-accepted course transcript (Accept with restrictions,
   paraphrase only) contains a claim shaped like "this pattern tends to
   precede breakouts" with no named source/date for that specific claim,
   and the pattern could plausibly inform a candidate predicate. Expect:
   the claim itself lands in Unsupported/needs-source claims (claim-audit
   would mark it Needs-source), and if it is extracted as predicate
   material at all, it appears only in EntryLens candidate material
   prefixed `HUMAN-REVIEW ONLY:` with no probability/edge/win-rate framing
   added → verdict **Ready for human review** (assuming claim-audit ran
   on the source's other claims), with the unsupported claim and the
   HUMAN-REVIEW ONLY label both visible for the human reviewer to weigh.

5. **Source-quality-accepted source with an unaudited claim** — an
   academic paper, source-quality verdict **Accept for claim-audit**
   (reliability high, use permission cite/use), containing a specific
   factual claim ("X occurs in Y% of historical cases") that has not yet
   been run through claim-audit in this session. Expect: the claim cannot
   be marked Supported (no claim-audit pass exists for it yet) and is not
   silently downgraded into Unsupported either — the skill stops before
   finishing extraction → verdict **Needs claim-audit first**, one-line
   reason naming the specific claim(s) still needing a claim-audit pass.

## What this skill must never do

- Store raw transcript, screenshot, clip, upload, audio, video, or other
  copyrighted raw material in any file, anywhere in the repo, regardless
  of source-quality use permission.
- Produce anything other than transformed, paraphrased, sanitized notes —
  no verbatim long-form reproduction of source material.
- Recommend a trade, setup, entry, exit, call, put, contract, sizing,
  stop, target, probability, edge, win rate, or expected return, in any
  bucket, under any framing.
- Compute or authorize EntryLens Green, or imply Green is satisfied,
  close, or likely.
- Write directly to `00_System/Master-Blueprint-V1.md`, `CLAUDE.md`,
  `OPERATING_DOCTRINE.md`, any `.claude/rules/**` file, any
  `03_EntryLens/**` or `04_AlphaLab/**` file, `00_System/Claim-Index.md`,
  or `00_System/Decision-Index.md`.
- Promote a candidate predicate, candidate rule, candidate replay-fixture
  idea, or product hypothesis into implementation, runtime, or any
  wired-in system — every item in those buckets stays HUMAN-REVIEW ONLY
  and proposal-only.
- Run on a source with no prior source-quality verdict, or treat any
  claim as Supported without a claim-audit pass behind it.
- Edit any file by default — output is for human review only.
