---
read_first: transcript-policy.md
source_of_truth: Master-Blueprint-V1.md §7
---

# YouTube-Ingest Workflow (one transcript at a time)

A manual operating runbook that **sequences existing pieces** into a single,
repeatable, one-transcript-at-a-time procedure. It composes — it does not
replace — the canonical artifacts:

- the **skill** [`.claude/skills/youtube-ingest/SKILL.md`](../.claude/skills/youtube-ingest/SKILL.md)
  (read-first advisory transform; defines the output schema),
- the **template** [`Templates/youtube-ingest.md`](../Templates/youtube-ingest.md)
  (the writable pipeline checklist), and
- the four policy files in this folder: [`transcript-policy.md`](transcript-policy.md),
  [`source-quality-rules.md`](source-quality-rules.md),
  [`lesson-template.md`](lesson-template.md), [`promotion-rules.md`](promotion-rules.md).

On any conflict, those files win over this one. This runbook adds nothing to the
automation ladder: it is **manual prompts/skills only** — no script, hook,
routine, Dispatch, or MCP.

## What this runbook is not

Not an ingester, not engine code, not automation, and not a second copy of the
skill's schema. It introduces no buy/sell/calls/puts/entries/exits/signal/scanner
behavior, never authorizes or redefines EntryLens Green, and never writes raw
transcript/clip/media to a tracked path.

**Green, verbatim** (reprinted unparaphrased wherever Green is referenced, per
[`.claude/rules/entrylens-trading-safety.md`](../.claude/rules/entrylens-trading-safety.md)):

> Green = every required condition in the trader's locked declared plan is
> deterministically satisfied right now. Green does not mean buy, trade, enter,
> high probability, edge, or best time.

No step in this runbook computes, authorizes, or overrides Green. Green is
deterministic-engine output only, and that engine does not live in this repo.

---

## 1. One-transcript-at-a-time process

Process **exactly one** transcript end to end. A transcript must fully clear (or
stop) before the next begins — no batching, no parallel sources.

| Step | Gate / action | Stop condition |
|---|---|---|
| 0 | **Direct-request hard stop** (skill step 1) | If the *request itself* asks for trade advice/signal, Green authorization, raw-transcript storage, transcript reproduction beyond a brief fair-use snippet, or media download → **stop**, cite [`transcript-policy.md`](transcript-policy.md) / `entrylens-trading-safety.md`. |
| 1 | **Rights/IP gate** (green/amber/red) on the raw transcript — [`00_System/Rights-IP-Gate.md`](../00_System/Rights-IP-Gate.md), [`transcript-policy.md`](transcript-policy.md) step 2 | **Red → stop**, delete the raw transcript, log a one-line reason in `rejected/`. |
| 2 | **`source-quality`** on the source | Reject / Human-review only / Block for safety → **stop** (skill won't run). |
| 3 | **`youtube-ingest` skill** — produces the advisory note; **invokes `claim-audit` internally** on every factual/performance claim | Needs source-quality first / Needs claim-audit first / Blocked → **stop**. |
| 4 | **`trading-safety-review`** on the produced note | Request changes / Block → **stop**, fix or drop the flagged item, re-run. |
| 5 | **`product-scope-review`** on the produced note | Needs trading-safety-review / Needs ADR / Defer / Out of scope-block → **stop**, route accordingly. |
| 6 | **Human promotion decision** via [`promotion-rules.md`](promotion-rules.md) | Promotion is always manual and optional. Clearing the gates never auto-promotes anything. |

Steps 0–1 are doctrine-required pre-gates that the bare four-gate chain
(`source-quality → claim-audit → trading-safety-review → product-scope-review`)
omits; they are mandatory here.

## 2. Where raw transcript text may temporarily exist

- **In-session only.** The advisory skill path writes nothing to disk — paste the
  transcript into the session, process it, keep nothing.
- If text *must* touch disk, the **only** permitted location is
  `06_YouTube-Lesson-Library/raw-transcripts/` — gitignored, local-only scratch
  that may be wiped at any time ([`transcript-policy.md`](transcript-policy.md)).
- Never paste raw transcript into any tracked path (no `inbox/`, no `packets/`,
  no note body, no commit message). **Extract, don't archive.**

## 3. Raw transcripts are never committed

Confirmed and enforced. `.gitignore` carries `**/raw-transcripts/`, so that
folder's contents can never be staged. Before any commit on a transcript run:

- run `git status --porcelain` and confirm **no** `raw-transcripts/` path and
  **no** transcript body appear in the diff;
- do **not** add a `.gitkeep` (or any file) to `raw-transcripts/` — the folder
  stays untracked by design.

The raw transcript is never the brain. If a packet/lesson is lost, re-deriving it
from a local raw transcript is acceptable; the raw transcript itself is never the
durable record.

## 4. Sanitized note destination

| Output | Destination | Notes |
|---|---|---|
| Transformed note (metadata + paraphrased notes) | `06_YouTube-Lesson-Library/packets/` | Never a verbatim transcript dump; quote only where fair-use-defensible. |
| Durable lesson | `06_YouTube-Lesson-Library/lessons/` | Uses [`lesson-template.md`](lesson-template.md). |
| Audited claims | [`00_System/Claim-Index.md`](../00_System/Claim-Index.md) | Human step; only claim-audit-passed claims. |
| EntryLens-adjacent candidates | [`03_EntryLens/Predicate-Candidates/`](../03_EntryLens/Predicate-Candidates/_index.md) | **Only** via the separate EntryLens promotion lock, never directly from ingest. |

It is **not** `07_Research-Library/` (that's non-YouTube general staging) and
**not** any EntryLens runtime surface. Terminology: a YouTube source becomes a
**lesson/packet** here; only a labeled *deterministic predicate* subset ever
reaches EntryLens, and only by promotion. The pipeline subfolders are created on
first write — except `raw-transcripts/`, which stays local/untracked. Per the
current task constraint, **do not write any packet/lesson/source note until
explicitly approved.**

## 5. Exact command/prompt for running youtube-ingest

Skills are human-invoked. Run, in order:

1. `/source-quality` — paste/describe the source; record the verdict and date.
2. `/youtube-ingest` — supply the source-quality verdict inline, e.g.:

   > `/youtube-ingest` — "Ingest this transcript, one source. source-quality
   > verdict: **Accept with restrictions** (paraphrase only), 2026-06-20. Run
   > `claim-audit` on every factual/performance claim. Reference: <title / creator
   > / URL>. [Transcript pasted for in-session processing only — not to be
   > committed or stored.]"

   The skill stops with **Needs source-quality first** if no verdict is supplied,
   and **Needs claim-audit first** if a claim needs auditing and claim-audit
   hasn't run.
3. `/claim-audit` — if any claim still needs an explicit audit pass.
4. `/trading-safety-review` — on the produced note.
5. `/product-scope-review` — on the produced note.

## 6. Required labels — HUMAN-REVIEW ONLY

Every EntryLens-adjacent line in the note lives under the schema section
`## EntryLens candidate material — HUMAN-REVIEW ONLY` and is prefixed
`HUMAN-REVIEW ONLY:`. Sub-types:

- `HUMAN-REVIEW ONLY: candidate predicate — <observable, falsifiable condition>`
- `HUMAN-REVIEW ONLY: candidate replay-fixture idea — <historical scenario to test>`
- `HUMAN-REVIEW ONLY: product hypothesis — <idea>`

These are **staged proposals only — never wired in, never self-authorized**
(see [ADR-0001](../08_Decision-Logs/ADR-0001-trader-intelligence-company.md)).
A candidate line is never a trade recommendation, entry/exit, contract, strike,
sizing, stop, target, or probability/edge/win-rate/expected-return claim. If an
item can only be stated as advice, it is dropped to Rejected/no-use instead.

## 7. Downstream gate order

`source-quality → claim-audit → trading-safety-review → product-scope-review`
(with the step 0–1 pre-gates ahead of it). Verdict sets and pass-conditions:

| Gate | Verdicts | Proceed only on |
|---|---|---|
| `source-quality` | Accept for claim-audit · Accept with restrictions · Human-review only · Reject · Block for safety | an **Accept\*** |
| `claim-audit` (internal to the skill) | Pass · Pass with caveats · Block until sourced · Block for safety | only Pass / Pass-with-caveats claims may be marked **supported** |
| `trading-safety-review` | Pass · Pass with cautions · Request changes · Block | **Pass / Pass with cautions** |
| `product-scope-review` | In scope · In scope with constraints · Needs trading-safety-review · Needs ADR · Defer · Out of scope / block | **In scope / In scope with constraints** |

Run `trading-safety-review` **before** `product-scope-review` (safety before
shape). The two are companions and cross-flag each other: a trade-/signal-/Green-/
broker-adjacent finding in scope review sets *Trading-safety-review needed = yes*,
and product-scope drift in the safety review names `product-scope-review`. Scope
review judges *shape*, not *safety*, and never clears trade/signal/Green/broker
content on its own.

## 8. Worked example — generic momentum-trading video (hypothetical, no real ingest)

**Source (paraphrased):** a creator describes a "momentum continuation" idea —
price breaking above a prior swing high on rising volume — and says *"I buy the
breakout, target the next resistance, this works about 70% of the time."*

- **Step 1 — Rights/IP gate →** amber (video, restricted-use-by-default;
  paraphrase only).
- **Step 2 — `source-quality` →** Accept with restrictions (paraphrase only).
- **Step 3 — `youtube-ingest` skill:**
  - *Paraphrased summary:* creator describes a breakout-above-prior-high
    momentum-continuation concept with volume confirmation.
  - *Timestamped concept:* `~04:10 — breakout-above-prior-high concept with volume
    (paraphrased)`.
  - *"~70% of the time"* → `claim-audit` **Block until sourced** + probability/edge
    safety flag → filed under **Unsupported / needs-source**, never adopted.
  - *"I buy the breakout / target the next resistance"* → a trade recommendation
    with entry/target → **dropped to Rejected / no-use** (cannot be stated without
    trade-recommendation framing).
  - *Useful patterns (non-advisory):* "breakout above a prior high with rising
    volume is a commonly cited momentum-continuation *concept*" — descriptive only.
  - *EntryLens candidate material — HUMAN-REVIEW ONLY:*
    - `HUMAN-REVIEW ONLY: candidate predicate — completed-bar close above the most
      recent prior swing high (observable, falsifiable; a structural condition, not
      a buy signal)`
    - `HUMAN-REVIEW ONLY: candidate replay-fixture idea — historical bars where a
      completed bar closed above a prior swing high with volume above its N-bar
      average (observation only; no outcome/edge claim)`
  - *Verdict:* **Ready for human review** (gates cleared; advice + edge claims
    dropped; real, safe content remains).
- **Step 4 — `trading-safety-review` →** Pass with cautions (caution: the dropped
  win-rate and buy/target language must stay dropped; the predicate is phrased as a
  condition, not a signal).
- **Step 5 — `product-scope-review` →** In scope with constraints (constraint: the
  predicate candidate may advance only via the EntryLens promotion lock; it must
  not become a live scanner, alert, or signal).

**Teaching point:** the raw *"buy the breakout, 70% win rate"* material is
transformed into a deterministic, observable predicate **candidate** held
HUMAN-REVIEW ONLY — the advice and edge claims are dropped, not laundered through
"candidate" language.

## 9. Definition of Done

**Runbook DoD (this file):** exists at `06_YouTube-Lesson-Library/workflow.md`;
sequences the full chain including the two pre-gates; cites (does not copy) the
skill/template/policy files; duplicates no schema; introduces no engine code,
automation, or template rewrite.

**Per-transcript-run DoD:**

- [ ] Direct-request hard stop cleared.
- [ ] Rights/IP gate run and not Red.
- [ ] `source-quality` returned an Accept\* verdict (recorded).
- [ ] `claim-audit` run on every factual/performance claim; only passed claims
      marked supported.
- [ ] `trading-safety-review` = Pass / Pass with cautions.
- [ ] `product-scope-review` = In scope / In scope with constraints.
- [ ] `git status` shows **zero** raw transcript / `raw-transcripts/` path
      committed.
- [ ] Every EntryLens candidate line prefixed `HUMAN-REVIEW ONLY:`.
- [ ] Green, if referenced anywhere, appears verbatim.
- [ ] Sanitized note lands only in `packets/` (and `lessons/` if promoted);
      nothing written to `03_EntryLens/` except via the EntryLens promotion lock.
- [ ] Any promotion done manually via [`promotion-rules.md`](promotion-rules.md).

## 10. Evidence to record in a proof log after the first real run

Model the entry on
[`09_Proof-Logs/product-scope-review-first-live-runs.md`](../09_Proof-Logs/product-scope-review-first-live-runs.md).
Capture:

- `id`, `date`, `linked_decision: ADR-0001`.
- Source identity — title / creator / date, **reference only; no raw transcript**.
- Rights-gate color and `source-quality` verdict.
- `claim-audit` tallies: supported / unsupported / blocked.
- Which advice/edge claims were correctly **dropped** (e.g. win-rate, buy/target
  instructions).
- `trading-safety-review` and `product-scope-review` verdicts.
- Confirmation no raw path was committed (paste the clean `git status` evidence).
- Confirmation every EntryLens candidate line is HUMAN-REVIEW ONLY.
- Green reproduced verbatim if it was referenced.
- Destination actually written (`packets/`, and `lessons/` if promoted).
- Verdict (usable / pass), known limitations, and the recommended next proof.
- The matching `00_System/Proof-Index.md` ledger row (add it, or record it as a
  deferred follow-up if that file is outside the run's allowed-file scope).
