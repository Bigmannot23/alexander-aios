---
name: source-quality
description: Human-invoked source classifier for Alexander-AIOS. Trigger when the user asks to "check this source," "evaluate this source," "source-quality this," "is this source any good," "should I ingest this," "can I use this article/video/course/paper/transcript," or before any article, video, course, transcript, paper, social post, or research link is treated as usable input, ingested, or sent to claim-audit. Given a pasted/attached source (or its description), records source identity, classifies source type and reliability, checks rights/use restriction, scans for safety flags (trading-advice, AI-authorized Green, broker/account/P&L risk, copyright/raw-transcript risk, marketing/hype, staleness), and recommends a next step. Lighter and earlier than the 0–20 Source-Quality-Rubric score — this is the qualitative gate that runs before or alongside it. Outputs one of five verdicts: Accept for claim-audit, Accept with restrictions, Human-review only, Reject, Block for safety. Never edits files by default, never ingests, never writes source notes, never stores raw transcripts/media.
---

# source-quality

`source-quality` is a **read-first, human-invoked classifier**. Given a
pasted source, an attached source, or a description of one, it records
source identity, classifies source type and reliability, checks rights/use
restriction, scans for safety flags, and recommends a next step. It runs
*before* a source is ingested or sent to `claim-audit` — `claim-audit`
audits claims already extracted from a draft; `source-quality` decides
whether the raw source is even worth that step. It never edits files by
default, never performs ingestion, never writes a source note, never
stores raw transcript/video/audio, and never computes the 0–20
`Source-Quality-Rubric.md` score itself — that stays a separate, manual
step if this skill's verdict allows progression.

## Source of truth — read only

This skill reads, but never edits during routine invocation:

- `.claude/rules/research-source-quality.md` — source-type/date/author/
  category capture, rights-gate discipline, four-bucket discipline
- `.claude/rules/entrylens-trading-safety.md` — trading-safety boundary and
  the verbatim Green definition
- `08_Decision-Logs/ADR-0001-trader-intelligence-company.md` — names
  source-quality review as permitted, human-review-only output; Claude
  never acts, ingests, or authorizes on its own
- `00_System/Source-Quality-Rubric.md` — the separate 0–20 numeric score;
  this skill is the lighter qualitative gate that runs earlier/alongside
  it, not a replacement
- `00_System/Rights-IP-Gate.md` — green/amber/red rights classification,
  independent of quality
- `claim-audit` (`.claude/skills/claim-audit/SKILL.md`) — the next gate
  once a source clears this one and its claims need auditing

The blueprint wins on every conflict. This read-only rule governs the
skill's own classifying behavior — it is distinct from the one-time
authoring step that created this file and its index entries.

## Procedure

Run in order against the supplied source:

1. **Identity capture** — title, author/provider, date or a freshness
   estimate, URL/location if given. If any field is unknown, record "not
   given" — never leave it blank.

2. **Source type classification** — exactly one of: official/primary,
   documentation, academic/research, legal/regulatory, expert
   practitioner, vendor/marketing, course/video/podcast,
   social/forum/community, unknown.

3. **Reliability pass** — high / medium / low / reject, using the same
   credibility / evidence / specificity / incentive-hygiene signals
   `Source-Quality-Rubric.md` scores numerically, without computing the
   0–20 number itself.

   **Boundary rule:** reliability = **reject** requires *both* an
   evidence failure (no data, no track record) *and* an incentive-hygiene
   failure (selling a signal/performance claim) together. A source with
   only one of those — e.g. low evidence but no monetary incentive, or a
   named author with a partial, checkable track record — scores
   reliability = **low**, not reject.

4. **Rights/use-restriction pass** — exactly one of: cite/use, paraphrase
   only, human-review only, do not ingest, reject. Cross-reference
   `Rights-IP-Gate.md`'s green/amber/red if the source's rights status is
   known, but record this skill's own five-way label as the field value.

5. **Safety scan** — flag any of: no date, stale source, marketing/hype,
   unverifiable claims, copyright/raw-transcript risk, trading-advice
   risk, broker/account/P&L/payment/banking risk, AI-authorized Green
   risk. If Green is referenced at all, reprint the verbatim definition
   from `entrylens-trading-safety.md`.

   **Critical distinction — restricted rights vs. unsafe raw-storage
   risk:** a source being paraphrase-only, amber, or otherwise
   restricted-use is recorded *only* in the Rights/use restriction field
   (step 4) — it is not, by itself, a safety flag. The
   `copyright/raw-transcript risk` safety flag fires *only* when the
   source itself requests, implies, or requires storing, reproducing,
   downloading, or scraping raw transcript/media — an action that would
   violate `repo-hygiene.md`'s raw-data ban or the rights gate — not
   merely because the source's use is restricted to paraphrase. A
   legitimately accessed, restricted-use source with no raw-storage
   request gets a clean safety-flags section and is evaluated on
   reliability/rights alone.

6. **Next-step recommendation** — exactly one of: reject, claim-audit
   next, research-ingest next, youtube/course-ingest next, decision-log
   candidate, no action.

7. **Verdict assignment** — via the precedence table below.

## Output schema

```
## Source Quality — <one-line description of source>

### Source identity
- Title:
- Author/provider:
- Date / freshness:
- URL/location:
(if any field is unknown, write "not given" — never leave blank)

### Source classification
- Type: official/primary | documentation | academic/research | legal/regulatory | expert practitioner | vendor/marketing | course/video/podcast | social/forum/community | unknown

### Reliability assessment
- Reliability: high | medium | low | reject
- Why: <one line>

### Rights/use restriction
- Use permission: cite/use | paraphrase only | human-review only | do not ingest | reject
- Why: <one line, cross-reference Rights-IP-Gate green/amber/red if known>

### Safety flags
(list each that applies; omit section entirely if none)
- no date / stale source / marketing-hype / unverifiable claims / copyright-raw-transcript risk / trading-advice risk / broker-account-P&L-payment-banking risk / AI-authorized-Green risk
(reprint verbatim Green definition if Green referenced at all)

### Recommended next step
reject | claim-audit next | research-ingest next | youtube/course-ingest next | decision-log candidate | no action

### Verdict: Accept for claim-audit | Accept with restrictions | Human-review only | Reject | Block for safety
<one-line reason>
```

## Verdict precedence

Evaluate top to bottom; the first match wins:

1. **Block for safety** — any safety flag is trading-advice risk,
   broker/account/P&L/payment/banking risk, or AI-authorized-Green risk.
2. **Reject** — reliability = reject (evidence failure + incentive-hygiene
   failure together — see the boundary rule in step 3), or use permission
   = reject, or copyright/raw-transcript risk is flagged at high severity
   (an explicit request or clear implication to store, redistribute, or
   reproduce copyrighted raw material).
3. **Human-review only** — copyright/raw-transcript risk is flagged at
   lower/ambiguous severity (unclear whether raw storage is actually
   implied), or use permission = do not ingest, or reliability = low (some
   educational value, but unreliable) with no higher-severity blocker.
4. **Accept with restrictions** — use permission = paraphrase only,
   reliability medium or high, **no safety flags** (no raw-storage/
   reproduction request present). This is the normal outcome for a
   legitimately accessed, restricted-use source.
5. **Accept for claim-audit** — reliability high or medium, use permission
   cite/use, no safety flags, recommended next step is claim-audit next or
   research/course/youtube-ingest next.

## Test cases

1. **Primary/official, dated, clean** — a regulator's published rule text,
   dated, named author/agency, no hype, no restrictions. Reliability
   high, use permission cite/use, no safety flags → verdict **Accept for
   claim-audit**.
2. **Vendor signal-selling video, undated, performance claim, no
   evidence** — sells a signal service, claims "80% win rate," no date, no
   track record, no author credibility shown (evidence failure +
   incentive-hygiene failure together, per the boundary rule). Type
   vendor/marketing, reliability **reject**, flags: no date +
   marketing/hype + unverifiable claims → verdict **Reject**, next step
   reject. *(Boundary contrast, not a separate test case: the same video
   with a named author and a partial, checkable track record would score
   reliability **low** instead of reject, and verdict **Human-review
   only**.)*
3. **Paid course transcript, legitimately accessed, no raw-storage
   request** — useful content; rights status is amber per
   `Rights-IP-Gate.md` (paraphrase only, no raw transcript retained). The
   request is to evaluate the source for use, not to store or reproduce
   the transcript. Reliability medium, use permission **paraphrase
   only**, safety flags: **none** (no raw-storage/reproduction requested)
   → verdict **Accept with restrictions**, next step research-ingest next,
   transformed notes only — raw transcript must not be stored.
4. **Forum post with trading advice + Green claim** — "this setup wins 80%
   of the time, Green is basically guaranteed here." Flags: trading-advice
   risk + AI-authorized-Green risk → reprint the verbatim Green
   definition → verdict **Block for safety**, next step reject.
5. **Undated blog post, stale market-structure claims** — no date, claims
   about market regime that may no longer hold, no way to verify
   currency. Flags: no date + stale source + unverifiable claims,
   reliability low (some content, but unconfirmable currency, no
   incentive-hygiene failure so not reject) → verdict **Human-review
   only**, next step decision-log candidate only if used to record the
   caveat, otherwise no action.

## What this skill must never do

- Edit any file by default — output is for human review only.
- Perform ingestion of any source, transcript, or course material.
- Write a source note or store raw transcript/video/audio.
- Compute the 0–20 `Source-Quality-Rubric.md` score itself — that stays a
  separate, manual step if the verdict allows progression.
- Compute or authorize EntryLens Green.
- Soften a safety-risk flag into "educational" framing that still
  functions as advice.
