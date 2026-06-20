---
name: trading-safety-review
description: 'Human-invoked read-first safety reviewer for Alexander-AIOS. Trigger when the user asks to "safety-review this," "trading-safety-review this," "check this for trading-safety drift," or before any text, plan, skill, research note, product idea, PR diff, or content draft that touches markets/EntryLens enters source-of-truth or ships (often after `route-task` routes an artifact as trading-adjacent). Audits the artifact line/item by line/item for trade recommendations, buy/sell language, calls/puts recommendations, entry/exit instructions, contracts/strikes/expirations, sizing/stops/targets, probability/edge/win-rate/expected-return claims, market-scanner behavior, alert/signal behavior, broker/account/order/P&L/payment/banking surfaces, AI-trading-assistant behavior, Claude-authorized EntryLens Green, Green redefined as buy/sell/edge/probability/approval, and user-personalized securities advice. Distinguishes safe plan-alignment language, human-review candidate language, unsafe recommendation/signal language, product-scope drift, and legal/compliance caution. Outputs line-level findings, a severity per finding (pass / caution / request changes / block), exact replacement wording, a verbatim Green-definition check, and a final verdict (Pass / Pass with cautions / Request changes / Block). Never edits files by default, never recommends trades, never authorizes or computes EntryLens Green, never ingests, never writes source notes, never stores raw material.'
---

# trading-safety-review

`trading-safety-review` is a **read-first, human-invoked safety reviewer**.
Given any artifact — pasted text, a plan, a skill definition, a research
note, a product idea, a PR diff, or a content draft — it scans line by line
(or item by item) for drift across the EntryLens trading-safety boundary,
classifies each finding, assigns a severity, supplies exact safe
replacement wording, and ends with one of four verdicts. It is itself
non-advisory: it never edits files by default, never recommends trades,
never ingests or writes source notes, never stores raw material, and never
computes or authorizes EntryLens Green. It may be invoked directly by a
human, or by `route-task` when an incoming artifact is trading-adjacent.

## Source of truth — read only

This skill reads, but never edits during routine invocation:

- `.claude/rules/entrylens-trading-safety.md` — the full trading-safety
  boundary and the verbatim Green definition
- `.claude/rules/research-source-quality.md` — the four-bucket discipline
  (facts, recommendations, assumptions, open questions) that separates *a
  source's own recommendation* from *advice the artifact is giving*
- `00_System/Master-Blueprint-V1.md` §1 (EntryLens status doctrine) / §6
  (universal trading boundary)
- `00_System/Safety-Policy.md` — the repo trading boundary
- `08_Decision-Logs/ADR-0001-trader-intelligence-company.md` — what Claude
  is allowed to do and what it must never become (trader, signal engine,
  scanner, broker/account/order/P&L/payment/banking surface, market
  automation agent, trade-recommendation system, EntryLens Green authority)

The blueprint wins on every conflict. This read-only rule governs the
skill's own reviewing behavior — it is distinct from the one-time authoring
step that created this file and its index entries.

## Procedure

Run in order against the supplied artifact:

1. **Classify the artifact** — record its type (text / plan / skill
   definition / research note / product idea / PR diff / content draft),
   its source or location, and whether it is trading-adjacent at all.
   Read-first; edit nothing.

2. **Quote-context gate (run before judging any severity).** For every
   unsafe-looking phrase, first decide whether it appears as **live
   content** (the artifact itself recommending, instructing, claiming, or
   building) or **quoted only as something to reject** — a negative
   example, a test fixture, a rules file's own prohibition list, a quote
   the artifact is explicitly forbidding, or this skill quoting itself.
   Material that is quoted-only-to-reject is flagged as *present-but-safe*
   and **never fires the hard-stop**. This gate is what stops the reviewer
   from blocking the rules files, ADR-0001, the sibling skills, and itself
   — all of which necessarily quote forbidden language in order to ban it.
   When in doubt about which it is, treat it as live content and downgrade
   only on clear evidence the artifact is rejecting it.

3. **Line/item-level scan** — walk the artifact and flag every hit against
   the 14 risk categories below, recording a precise location for each
   (line number, list item, or diff hunk):
   1. trade recommendations
   2. buy/sell language
   3. calls/puts recommendations
   4. entry/exit instructions
   5. contracts / strikes / expirations
   6. sizing / stops / targets
   7. probability / edge / win-rate / expected-return claims
   8. market-scanner behavior
   9. alert / signal behavior
   10. broker / account / order / P&L / payment / banking surfaces
   11. AI-trading-assistant behavior (a model acting as a trade adviser/agent)
   12. Claude-authorized (or any-model-authorized/computed) EntryLens Green
   13. Green redefined to mean buy / sell / edge / probability / approval
   14. user-personalized securities advice ("for your account, you should…")

4. **Classify each finding** into exactly one of the five distinctions
   (see *The five distinctions* below): safe plan-alignment · human-review
   candidate · unsafe recommendation/signal · product-scope drift ·
   legal/compliance caution.

5. **Green definition check** — if Green is referenced anywhere in the
   artifact, reprint the verbatim definition (below) and verify the
   artifact does **not** redefine Green as buy/trade/enter/edge/
   probability/best-time/approval, and does **not** have Claude or any
   other model computing or authorizing it. Green is deterministic-engine
   output only.

   > **Green = every required condition in the trader's locked declared
   > plan is deterministically satisfied right now. Green does not mean
   > buy, trade, enter, high probability, edge, or best time.**

6. **Assign a severity per finding** — pass / caution / request changes /
   block (rules in *Severity and verdict rules*).

7. **Required replacements** — for every unsafe or fixable finding, give
   exact safe replacement wording: rewrite advice as descriptive
   plan-alignment language, drop a premature proposal to a
   `HUMAN-REVIEW ONLY:` candidate, or remove the surface entirely. Show the
   before → after explicitly so a human can apply it.

8. **Product-scope drift note** — if any finding is product-scope drift,
   describe the drift and name `product-scope-review` as the next/companion
   skill for the scope decision. (That skill is **not built yet** — name it
   anyway so the referral is recorded.)

9. **Final verdict** by precedence (see *Severity and verdict rules*).

## The five distinctions

- **Safe plan-alignment** — descriptive statements about whether a
  trader's own locked, declared plan conditions are deterministically
  satisfied right now, with no buy/sell/enter framing, no claim of edge or
  probability, and no model authorizing anything. Example of safe:
  "the declared invalidation level has not been breached." Severity
  **pass**.
- **Human-review candidate** — predicate / rule / replay-fixture /
  hypothesis ideas explicitly labeled `HUMAN-REVIEW ONLY`, staged and not
  wired into any runtime, per ADR-0001's "staged proposals only, never
  self-authorized" boundary. Correctly labeled → **pass**; the same
  material presented as a decision or as already-active → **request
  changes** (relabel) or **block** if it also recommends a trade.
- **Unsafe recommendation/signal** — any instruction or claim that
  functions as advice or a tradeable signal: buy/sell/entry/exit,
  calls/puts, contracts/sizing/stops/targets, edge/probability/win-rate/
  expected-return, a scanner/alerter that surfaces "act now" output, an AI
  acting as trade adviser, or any model authorizing Green. Severity
  **block** when live (see *Hard-stop*).
- **Product-scope drift** — the artifact pushes EntryLens or AIOS toward
  becoming a trader, scanner, signal engine, broker/execution surface, or
  advisory product, as a *proposal or design direction* rather than as a
  live trade call. Route to `product-scope-review`; severity **caution**
  (mild/ambiguous framing) or **request changes** (a concrete proposal to
  build out-of-scope behavior). If the proposal already emits buy/sell
  signals as live content, it is unsafe recommendation/signal → **block**,
  not drift.
- **Legal/compliance caution** — phrasing a regulator could read as
  investment advice, or a performance/suitability claim that would need
  disclaimers, without itself being a direct trade call. Severity
  **caution**; recommend human compliance review.

## Hard-stop

If the artifact, as **live content** (not quoted-only-to-reject per step
2), directly recommends trades, authorizes or computes Green, implies
buy/sell/entry/exit, includes contracts/sizing/stops/targets, claims
edge/probability/win-rate/expected return, or introduces broker/account/
order/P&L/payment/banking surfaces, the verdict is **Block**. The only
exception is the quote-context gate: unsafe material quoted purely as
something the artifact is rejecting is flagged *present-but-safe* and does
not trigger Block. Do not soften a live hard-stop hit into "educational"
framing that still functions as advice. Whenever Green is referenced at
all, reprint the verbatim definition:

> **Green = every required condition in the trader's locked declared plan
> is deterministically satisfied right now. Green does not mean buy, trade,
> enter, high probability, edge, or best time.**

## Output schema

````
## Trading-Safety Review — <one-line description of artifact>

### Artifact reviewed
- Type: text / plan / skill / research note / product idea / PR diff / content draft
- Trading-adjacent: yes / no
- Source/location: <path, PR #, or "pasted text">

### Safety verdict
<Pass | Pass with cautions | Request changes | Block> — <one-line reason>

### Summary
<2–4 sentences: what the artifact is and the headline safety result>

### Findings
| # | Location | Quoted phrase / paraphrased issue | Risk category | Class | Severity | Required fix |
|---|---|---|---|---|---|---|
| 1 | line 12 | "..." | buy/sell | unsafe rec/signal | block | <replacement> |
(write "No findings." instead of a table body if the artifact is clean; for quoted-only-to-reject hits, keep the matching Class but set Severity=pass and Required fix="None — quoted-only-to-reject")
### Green definition check
(if Green is not referenced, write: "Not applicable — Green not referenced.")
Reprint verbatim:
> **Green = every required condition in the trader's locked declared plan is
> deterministically satisfied right now. Green does not mean buy, trade,
> enter, high probability, edge, or best time.**
- Redefined as buy/sell/edge/probability/approval? yes/no
- Claude or any model computing or authorizing it? yes/no

### Product-scope drift notes
(write "None." if no drift; otherwise describe the drift and name
`product-scope-review` — not built yet — as the next/companion skill)

### Required replacements
(list each unsafe/fixable phrase → exact safe replacement wording; "None." if clean)

### Final verdict: Pass | Pass with cautions | Request changes | Block
<one-line reason>
````

## Severity and verdict rules

**Finding-level severity:**

- **pass** — safe plan-alignment language, a correctly-labeled
  `HUMAN-REVIEW ONLY` candidate, or unsafe text quoted-only-to-reject.
- **caution** — a legal/compliance caution, mild/ambiguous drift not yet
  functioning as advice, or stale safety-relevant framing. Non-blocking,
  but flag for a human.
- **request changes** — fixable phrasing that is not a live hard-stop
  category: mislabeled or unstaged candidate material, soft signal-ish or
  alert phrasing proposed as a feature, or a concrete product-scope-drift
  proposal needing a scope decision. Must be edited before the artifact
  proceeds.
- **block** — any hard-stop category present as **live content**: a trade
  recommendation, Green authorization/computation, buy/sell/entry/exit,
  contracts/sizing/stops/targets, edge/probability/win-rate/
  expected-return, or a broker/account/order/P&L/payment/banking surface.

**Artifact-level final verdict (evaluate top to bottom; first match wins):**

1. **Block** — any block-severity finding present as live content.
2. **Request changes** — no block, but at least one request-changes finding.
3. **Pass with cautions** — no block or request-changes, but at least one
   caution.
4. **Pass** — clean, or only pass-severity findings.

## Test cases

1. **Clean plan-alignment artifact** — a note describes, descriptively,
   whether a trader's locked declared plan conditions are deterministically
   met; no buy/sell, no contracts/sizing/stops, no edge/probability claim,
   no model authorizing Green. Expect "No findings." → verdict **Pass**.
2. **Legal/compliance caution** — a content/product draft makes a general
   market statement a regulator could read as investment advice and lacks
   any disclaimer, but issues no direct trade recommendation. Expect one
   `caution` finding classed legal/compliance → verdict **Pass with
   cautions**.
3. **Product-scope drift** — a product idea proposes that EntryLens
   auto-scan the market and push "act now" alerts. Expect findings on
   market-scanner behavior and alert/signal behavior, classed
   product-scope drift at `request changes`, with `product-scope-review`
   (not built yet) named as the next skill → verdict **Request changes**.
4. **Hard-stop breach (live content)** — artifact states e.g. "take the
   calls when RSI < 30, ~80% win rate, 2 contracts, stop at X, and Claude
   marks Green to approve entry; route the order to the linked account."
   Expect `block` findings across (a) calls + contracts/sizing/stops/
   targets, (b) probability/edge/win-rate, (c) broker/account/order
   surface, and (d) Claude-authorized Green, with the verbatim Green
   definition reprinted → verdict **Block**.
5. **Quoted-to-reject exception** — the artifact is itself a safety rule or
   test fixture that quotes "take the calls now, 90% win rate" *only* as an
   example of forbidden language it is banning. Expect that phrase flagged
   *present-but-safe* (quoted-to-reject), the hard-stop NOT fired → verdict
   **Pass** (this is the case that proves the reviewer will not block the
   rules files, ADR-0001, the sibling skills, or this skill itself).

## What this skill must never do

- Edit any file by default — output is for human review only.
- Itself recommend a trade, or produce buy/sell/entry/exit language as live
  guidance.
- Compute, authorize, or override EntryLens Green, or paraphrase the Green
  definition instead of reprinting it verbatim.
- Soften a live hard-stop finding into "educational" framing that still
  functions as advice.
- Perform ingestion, write source notes, or store raw transcript/media/
  screenshots/uploads.
- Promote a `HUMAN-REVIEW ONLY` candidate into implementation — flagging it
  as correctly staged is the most it does.
- Become a script, hook, routine, or Dispatch workflow — it stays a manual,
  human-invoked skill.
