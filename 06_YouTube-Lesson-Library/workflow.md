---
read_first: transcript-policy.md
source_of_truth: Master-Blueprint-V1.md §7
---

# YouTube-Ingest Workflow (one transcript at a time)

## What this is

A manual operating runbook that **sequences existing pieces** into a single,
repeat-by-hand procedure for turning one YouTube transcript at a time into a
sanitized, gated lesson. It composes — it does not replace — the canonical
artifacts:

- the `youtube-ingest` **skill** (`../.claude/skills/youtube-ingest/SKILL.md`) —
  the read-first advisory transformer that produces the note;
- the `youtube-ingest` **template** (`../Templates/youtube-ingest.md`) — the
  rights-gated, rubric-scored checklist that drives the writable pipeline;
- the four policy files in this folder (`transcript-policy.md`,
  `source-quality-rules.md`, `lesson-template.md`, `promotion-rules.md`);
- the five review skills: `/source-quality`, `/claim-audit`,
  `/youtube-ingest`, `/trading-safety-review`, `/product-scope-review`.

On any conflict, those canonical files win — this runbook is a sequence, not a
new source of truth.

## What this is not

Not automation. This stays at the current automation ceiling: **manual prompts
and skills only** — no script, hook, routine, Dispatch, or MCP. Not a second
copy of the skill's output schema (cited, never duplicated). Not EntryLens
runtime/engine code, and not a trade/signal/scanner surface of any kind.

## Read-first file

`transcript-policy.md`, then this file.

---

## Safety boundary (applies to every step)

- No trade recommendations, no buy/sell/calls/puts/entries/exits, no
  contracts/strikes/sizing/stops/targets, no probability/edge/win-rate/
  expected-return claims.
- No market-scanner, alert, or signal behavior. No broker/account/order/P&L/
  payment/banking surface.
- No model-computed or model-authorized EntryLens Green; Green is never
  redefined. Where Green is referenced, it is reprinted **verbatim**:

  > Green = every required condition in the trader's locked declared plan is
  > deterministically satisfied right now. Green does not mean buy, trade,
  > enter, high probability, edge, or best time.

- No raw transcript, clip, screenshot, audio, or video in any tracked path.
- Every EntryLens-adjacent candidate is `HUMAN-REVIEW ONLY:` and proposal-only,
  never wired in, never self-authorized (per
  `../08_Decision-Logs/ADR-0001-trader-intelligence-company.md`).

---

## 1. One-transcript-at-a-time process

Process exactly one transcript end to end before starting the next. Each step
can **stop** the run; a stop on one transcript never spills into another.

| # | Step | Tool / file | Stop condition |
|---|---|---|---|
| 0 | **Direct-request hard stop** | skill step 1 | User asks for trade advice/signal, Green authorization, raw-transcript storage, transcript reproduction beyond a brief fair-use snippet, or media download → refuse, do not proceed. |
| 1 | **Rights/IP gate (green / amber / red)** | `../00_System/Rights-IP-Gate.md`, `transcript-policy.md` | **Red** → delete the raw transcript, log a one-line reason in `rejected/`, stop. |
| 2 | **source-quality** | `/source-quality` | Verdict is Reject / Human-review only / Block for safety → stop (record why). |
| 3 | **youtube-ingest** (claim-audit invoked inside) | `/youtube-ingest` | Skill returns a blocked/needs-* verdict → stop and resolve the named gap first. |
| 4 | **trading-safety-review** | `/trading-safety-review` | Verdict Request changes / Block → stop, fix, re-run. |
| 5 | **product-scope-review** | `/product-scope-review` | Verdict Needs trading-safety-review / Needs ADR / Defer / Out of scope-block → stop and route accordingly. |
| 6 | **Human promotion decision** | `promotion-rules.md` | Operator alone decides; nothing auto-promotes. |

Steps 0–1 are mandatory pre-gates required by doctrine even though the
"download" chain only names steps 2–5.

## 2. Where raw transcript text may temporarily exist

- **In-session only** is the default: the advisory skill reads pasted text and
  writes nothing.
- If the text must touch disk, it goes **only** in
  `06_YouTube-Lesson-Library/raw-transcripts/` — gitignored, local-machine-only
  scratch space that may be wiped at any time.
- Never paste transcript text into any tracked file (no packet, lesson, note,
  index, or commit body). "Extract, don't archive."

## 3. Raw transcripts are never committed

Confirmed and enforced:

- `.gitignore` carries `**/raw-transcripts/` (plus `**/raw-screenshots/`,
  `**/raw-clips/`, `**/raw-uploads/`).
- **Pre-commit check:** run `git status --porcelain` and confirm no
  `raw-transcripts/` path and no transcript body appear in the staged set.
- Do **not** add a `.gitkeep` to `raw-transcripts/` — the folder stays untracked
  on purpose.
- The raw transcript is never the brain: if a packet/lesson is lost,
  re-deriving it from a local raw transcript is acceptable, but the raw
  transcript is never the durable record.

## 4. Sanitized note destination

- **Transformed note (packet)** — metadata + paraphrased notes only →
  `06_YouTube-Lesson-Library/packets/`.
- **Durable lesson** — built with `lesson-template.md` →
  `06_YouTube-Lesson-Library/lessons/`.
- **Audited claims** → logged to `../00_System/Claim-Index.md` (human step).
- **EntryLens-adjacent candidates** → reach
  `../03_EntryLens/Predicate-Candidates/_index.md` **only** via the separate
  EntryLens promotion lock, never directly from ingest. Accepting a YouTube
  lesson never, by itself, promotes anything into EntryLens.
- **Not** `07_Research-Library/` (that folder is for non-YouTube general
  staging) and **not** any EntryLens runtime location.

Terminology note: YouTube output is a **lesson/packet**, not an "EntryLens
note." Only a labeled, deterministic predicate subset ever becomes an EntryLens
*candidate*, and only through promotion.

First-run note: `packets/` and `lessons/` are created on first write;
`raw-transcripts/` stays local and untracked.

## 5. Exact command / prompt for running youtube-ingest

Run, in order, in Claude Code:

1. `/source-quality` — paste/describe the source; record the verdict and any
   use restriction.
2. `/youtube-ingest` — supply the source-quality verdict inline. Example:

   > `/youtube-ingest` — "Ingest this transcript, one pass.
   > source-quality verdict: Accept with restrictions (paraphrase only),
   > 2026-06-20. Run claim-audit on every factual/performance claim.
   > [paste the transcript for in-session processing only — do not write it to
   > any tracked file]."

   The skill stops with **Needs source-quality first** if no verdict is
   supplied, and **Needs claim-audit first** if a claim would be marked
   supported without an audit.
3. `/claim-audit` — only if you want a standalone audit pass in addition to the
   one the skill invokes internally.
4. `/trading-safety-review` — on the produced note.
5. `/product-scope-review` — on the produced note.

## 6. Required labels — HUMAN-REVIEW ONLY candidates

Inside the skill's `## EntryLens candidate material — HUMAN-REVIEW ONLY`
section, **every line** is prefixed `HUMAN-REVIEW ONLY:`. Permitted sub-types:

- `HUMAN-REVIEW ONLY: candidate predicate — <observable, falsifiable structural
  condition>`
- `HUMAN-REVIEW ONLY: candidate replay-fixture idea — <historical scenario to
  test the predicate; observation only>`
- `HUMAN-REVIEW ONLY: product hypothesis — <staged idea>`

These are staged proposals only: never wired in, never self-authorized, and
never a trade recommendation, entry/exit, contract/sizing/stop/target, or
probability/edge/win-rate claim. A candidate that can only be stated as advice
is dropped to Rejected/no-use, not laundered through "candidate" language.

## 7. Downstream gate order

`source-quality → claim-audit → trading-safety-review → product-scope-review`
(with the rights/IP gate and direct-request hard stop ahead of all of them, per
§1). Verdict sets and pass-conditions:

| Gate | Verdicts | Must be… to proceed |
|---|---|---|
| `source-quality` | Accept for claim-audit / Accept with restrictions / Human-review only / Reject / Block for safety | an **Accept\*** |
| `claim-audit` (inside the skill) | Pass / Pass with caveats / Block until sourced / Block for safety | only **Pass / Pass with caveats** claims may be marked supported |
| `trading-safety-review` | Pass / Pass with cautions / Request changes / Block | **Pass / Pass with cautions** |
| `product-scope-review` | In scope / In scope with constraints / Needs trading-safety-review / Needs ADR / Defer / Out of scope-block | **In scope / In scope with constraints** |

Run `trading-safety-review` **before** `product-scope-review` (safety before
shape). They are companions and cross-flag each other: a trade-/signal-/Green-/
broker-adjacent finding in scope review sets "Trading-safety-review needed? =
yes," and a scope-drift finding in safety review names `product-scope-review`.

## 8. Worked example — generic momentum-trading video (hypothetical)

No real ingest; illustration only.

**Source:** a creator describes a "momentum continuation" idea — price breaks
above a prior swing high on rising volume — and adds: *"I buy the breakout,
target the next resistance, this works about 70% of the time."*

1. **Rights/IP gate →** amber (video, restricted-use-by-default; paraphrase
   only). **source-quality →** Accept with restrictions.
2. **`/youtube-ingest` output:**
   - *Paraphrased summary:* creator describes a breakout-above-prior-high
     momentum-continuation concept with volume confirmation.
   - *Timestamped concept:* `~04:10 — breakout-above-prior-high concept with
     volume (paraphrased)`.
   - *"works ~70% of the time"* → claim-audit **Block until sourced** + a
     probability/edge safety flag → filed under Unsupported / needs-source,
     **never adopted**.
   - *"I buy the breakout / target resistance"* → a trade instruction →
     **dropped to Rejected / no-use** (would require trade-recommendation
     framing to state).
   - *Safe transformation* — the only extractable items, both labeled:
     - `HUMAN-REVIEW ONLY: candidate predicate — completed-bar close above the
       most recent prior swing high (observable, falsifiable; a structural
       condition, not a buy signal)`
     - `HUMAN-REVIEW ONLY: candidate replay-fixture idea — historical bars where
       a completed bar closed above a prior swing high with volume above its
       N-bar average (observation only; no outcome or edge claim)`
   - *Verdict:* **Ready for human review**.
3. **`/trading-safety-review` →** Pass with cautions (the dropped win-rate and
   buy instruction must stay dropped; predicate is phrased as a condition, not a
   signal).
4. **`/product-scope-review` →** In scope with constraints (the candidate may
   advance only via the EntryLens promotion lock; it must not become a live
   scanner or alert).

Net: a safe, paraphrased lesson plus two HUMAN-REVIEW ONLY candidates; the
advice and edge claims are gone; nothing is promoted without a separate manual
decision.

## 9. Definition of Done

**Runbook (this file):** exists at `06_YouTube-Lesson-Library/workflow.md`;
sequences the full chain including the two pre-gates; cites (does not copy) the
skill, template, and policy files; duplicates no schema; adds no engine code, no
automation, and no template rewrite.

**A single transcript run is done when:**

- the direct-request hard stop was cleared;
- the rights/IP gate ran and was not Red;
- `source-quality` returned an Accept\*;
- `claim-audit` ran on every factual/performance claim;
- `trading-safety-review` is Pass / Pass with cautions;
- `product-scope-review` is In scope / In scope with constraints;
- `git status` is clean of any raw path — no transcript committed;
- every EntryLens candidate line is prefixed `HUMAN-REVIEW ONLY:`;
- Green, if referenced, appears verbatim;
- the note lands only in `packets/` (and `lessons/` if promoted);
- any promotion went through `promotion-rules.md` with human approval;
- nothing was written to `03_EntryLens/` except via the EntryLens promotion
  lock.

## 10. Evidence to record in proof logs after the first real run

After the first real transcript run (not before), add a proof log under
`../09_Proof-Logs/`, modeled on
`../09_Proof-Logs/product-scope-review-first-live-runs.md`, recording:

- `id`, `date`, `linked_decision: ADR-0001`;
- source identity (title / creator / date) **by reference only — no raw
  transcript**;
- rights-gate color and `source-quality` verdict;
- claim-audit tallies: supported / unsupported / blocked;
- which advice or edge claims were correctly **dropped** (e.g. win-rate, buy
  instruction);
- `trading-safety-review` and `product-scope-review` verdicts;
- confirmation no raw path was committed (`git status` evidence);
- confirmation every candidate line is `HUMAN-REVIEW ONLY:`;
- Green reprinted verbatim if it was referenced;
- destination written (`packets/`, and `lessons/` if promoted);
- verdict (usable / pass), known limitations, and the recommended next proof;
- a follow-up note to add the matching row to `../00_System/Proof-Index.md`.
