---
read_first: transcript-policy.md
source_of_truth: Master-Blueprint-V1.md §7
---

# YouTube-Ingest Workflow (one transcript at a time)

A manual **operating procedure** that sequences the pieces that already exist —
the `youtube-ingest` skill, the `Templates/youtube-ingest.md` checklist, this
folder's policy files, and the downstream review skills — into a single,
repeatable, one-transcript-at-a-time flow.

This runbook **composes**; it does not replace. On any conflict the canonical
artifacts win, in this order: `00_System/Master-Blueprint-V1.md` →
`OPERATING_DOCTRINE.md` → this folder's policy files
(`transcript-policy.md`, `source-quality-rules.md`, `lesson-template.md`,
`promotion-rules.md`) → the skill definitions → this file. It introduces **no**
new schema, **no** engine code, and **no** automation: it stays at the current
ceiling — manual prompts and skills only (no script, hook, routine, Dispatch, or
MCP).

## What belongs here

The end-to-end procedure for turning **one** YouTube/video/course/podcast
transcript at a time into a sanitized, human-reviewed lesson — including the
mandatory pre-gates, the review chain, the destinations, and the
definition of done.

## What does not belong here

The schema, verdict logic, or boundaries of any skill (those live in each
`SKILL.md`); the writable pipeline mechanics (those live in
`Templates/youtube-ingest.md` and `promotion-rules.md`); raw transcripts or any
raw media (those never enter a tracked path — see `transcript-policy.md`).

## Read-first file

`transcript-policy.md`, then this file.

---

## Safety boundary (binding, read before every run)

This workflow never produces a trade recommendation, signal, scanner, alert, or
broker/account/P&L surface, and never computes, authorizes, or redefines
EntryLens Green. Where Green is referenced anywhere downstream, it is reprinted
verbatim and unparaphrased:

> Green = every required condition in the trader's locked declared plan is
> deterministically satisfied right now. Green does not mean buy, trade, enter,
> high probability, edge, or best time.

EntryLens runtime/engine code never lives in this repo. Everything trading-
adjacent that survives ingest is a **HUMAN-REVIEW ONLY** candidate, staged and
never wired in (per `08_Decision-Logs/ADR-0001-trader-intelligence-company.md`).

---

## 1. One-transcript-at-a-time process

Process exactly **one** transcript end to end. It must fully clear (or stop) at
every gate before the next transcript begins — no batching, no parallel runs.

| Step | Gate / action | Skill / file | Stop condition |
|---|---|---|---|
| 0 | **Direct-request hard stop** | `youtube-ingest` step 1 | Stop if the *request itself* asks for trade advice/signal, Green authorization, raw-transcript storage, transcript reproduction, or media download. |
| 1 | **Rights/IP gate** (green/amber/red) | `00_System/Rights-IP-Gate.md`, `transcript-policy.md` | **Red → stop:** delete the raw transcript, log to `rejected/`, end. |
| 2 | **source-quality** | `/source-quality` | Stop unless verdict is *Accept for claim-audit* or *Accept with restrictions*. |
| 3 | **youtube-ingest** (runs **claim-audit** internally) | `/youtube-ingest` | Stops itself with *Needs source-quality first* / *Needs claim-audit first* / a blocked verdict. |
| 4 | **trading-safety-review** | `/trading-safety-review` | Proceed only on *Pass* or *Pass with cautions*. |
| 5 | **product-scope-review** | `/product-scope-review` | Proceed only on *In scope* or *In scope with constraints*. |
| 6 | **Human promotion decision** | `promotion-rules.md` | Operator decides; rejection is expected and healthy. |

Steps 0–1 are doctrine-mandatory pre-gates that the bare four-skill chain does
not include — run them first, every time.

---

## 2. Where raw transcript text may temporarily exist

- **Preferred: in-session only.** The `youtube-ingest` skill is read-only and
  writes nothing; paste the transcript into the session, process it, and let it
  fall out of memory.
- **If it must touch disk:** `06_YouTube-Lesson-Library/raw-transcripts/`
  **only** — gitignored, local machine only, scratch space that may be wiped at
  any time. Never paste raw transcript text into any other path, and never into
  a tracked file.
- **Extract, don't archive.** The raw transcript is never the durable record; if
  a packet/lesson is lost, re-deriving it from the raw transcript is acceptable,
  but the raw transcript itself is never the brain (`transcript-policy.md`).

---

## 3. Raw transcripts are never committed

Confirmed and enforced:

- `.gitignore` carries `**/raw-transcripts/` (plus `**/raw-screenshots/`,
  `**/raw-clips/`, `**/raw-uploads/`), so the scratch folder and its contents are
  never staged.
- Do **not** add a `.gitkeep` (or any file) to `raw-transcripts/` — the folder
  stays untracked.
- **Pre-commit check (every run):** `git status --porcelain` must show **no**
  `raw-transcripts/` path and **no** pasted transcript body anywhere in the
  staged diff. If it does, unstage and remove before committing.
- Only **transformed, paraphrased** notes are ever committed. Quoting is limited
  to brief, fair-use-defensible snippets regardless of the rights determination.

---

## 4. Sanitized note destination

| Output | Destination | Notes |
|---|---|---|
| Transformed note (metadata + paraphrased notes) | `06_YouTube-Lesson-Library/packets/` | The "packet" stage — never a verbatim transcript dump. |
| Durable lesson | `06_YouTube-Lesson-Library/lessons/` | Uses `lesson-template.md`. |
| Audited claims | `00_System/Claim-Index.md` | Human step; only post-`claim-audit`. |
| EntryLens-adjacent candidates | `03_EntryLens/Predicate-Candidates/` | **Only** via the EntryLens promotion lock — never written directly from ingest. Every line `HUMAN-REVIEW ONLY:`. |

Not `07_Research-Library/` (that is for non-YouTube general staging), and never
any EntryLens runtime surface. Terminology note: YouTube output is a
**lesson/packet**, not an "EntryLens note." Only a labeled, deterministic
predicate subset ever reaches EntryLens, and only through promotion.

First-run note: the pipeline subfolders (`packets/`, `lessons/`, …) do not
physically exist yet; they are created on first write. `raw-transcripts/` is the
exception — it stays local and untracked.

---

## 5. Exact command / prompt for running youtube-ingest

Run these in order in Claude Code:

1. **Source-quality first:**
   ```
   /source-quality
   ```
   Supply the source identity (title, creator, date, URL/location, type) and the
   transcript reference. Record the verdict.

2. **Then youtube-ingest**, supplying the source-quality verdict inline (the
   skill stops with *Needs source-quality first* otherwise):
   ```
   /youtube-ingest
   ```
   > Ingest this transcript, one transcript only. source-quality verdict:
   > `Accept with restrictions (paraphrase only), 2026-06-20`. Run `claim-audit`
   > on every factual/performance claim before marking anything supported.
   > Paraphrase only; timestamps as concept pointers, not excerpts. [paste the
   > transcript for in-session processing — do not commit it].

3. **Then the downstream reviews** on the produced note:
   ```
   /trading-safety-review
   /product-scope-review
   ```
   (`/claim-audit` is invoked inside `youtube-ingest`; run it standalone only if
   you need to re-audit a specific claim.)

If a required input is missing, the skill returns *Needs source-quality first*
or *Needs claim-audit first* — supply it and re-run; do not work around it.

---

## 6. Required labels — HUMAN-REVIEW ONLY candidates

Every EntryLens-adjacent item lives under the skill's
`## EntryLens candidate material — HUMAN-REVIEW ONLY` section, and **every line**
carries the prefix `HUMAN-REVIEW ONLY:`. Sub-types:

- `HUMAN-REVIEW ONLY: candidate predicate — <observable, falsifiable, deterministic condition>`
- `HUMAN-REVIEW ONLY: candidate replay-fixture idea — <historical scenario to test the predicate; observation only>`
- `HUMAN-REVIEW ONLY: product hypothesis — <staged idea for human review>`

These are staged proposals only — never wired in, never self-authorized
(ADR-0001). A candidate line is **never** a trade recommendation, entry/exit,
contract/strike/expiration, sizing/stop/target, or probability/edge/win-rate/
expected-return claim. Anything that can only be stated in that form is dropped
to *Rejected / no-use*, not laundered through "candidate" language.

---

## 7. Downstream gate order

`source-quality → claim-audit → trading-safety-review → product-scope-review`
(with the doctrine pre-gates from §1 ahead of them). Verdict sets and
pass-conditions:

| Gate | Verdicts (verbatim) | Proceed only if |
|---|---|---|
| `source-quality` | Accept for claim-audit \| Accept with restrictions \| Human-review only \| Reject \| Block for safety | Accept for claim-audit / Accept with restrictions |
| `claim-audit` (internal) | Pass \| Pass with caveats \| Block until sourced \| Block for safety | Claim marked *supported* only on Pass / Pass with caveats |
| `trading-safety-review` | Pass \| Pass with cautions \| Request changes \| Block | Pass / Pass with cautions |
| `product-scope-review` | In scope \| In scope with constraints \| Needs trading-safety-review \| Needs ADR \| Defer \| Out of scope / block | In scope / In scope with constraints |

Run `trading-safety-review` **before** `product-scope-review` — safety before
shape. The two are companions and cross-flag each other: a finding that is
trade-/signal-/Green-/broker-adjacent sets *Trading-safety-review needed = yes*;
product-scope-review judges shape, not safety, and never clears trade/signal/
Green/broker content on its own.

---

## 8. Worked example — generic momentum-trading video (hypothetical)

> Illustrative only — no real ingest. Source: a creator describes a
> breakout-above-prior-swing-high "momentum continuation" concept and says
> *"I buy the breakout, target the next resistance, this works about 70% of the
> time."*

- **Step 1 — Rights/IP gate:** video, restricted-use-by-default → **amber**
  (paraphrase only; no verbatim beyond a fair-use snippet).
- **Step 2 — source-quality:** creator and date identified → **Accept with
  restrictions** (paraphrase only). (No date or pure marketing hype would →
  *Human-review only* → stop.)
- **Step 3 — youtube-ingest (claim-audit internal):**
  - *Paraphrased summary:* "Creator describes a momentum-continuation concept
    around a breakout above a prior swing high with rising volume."
  - *Timestamped concept:* `~04:10 — breakout-above-prior-high concept with
    volume confirmation (paraphrased)`.
  - *The "~70% of the time" claim* → claim-audit **Block until sourced** + a
    probability/edge safety flag → filed under *Unsupported / needs-source*,
    never marked supported, never adopted.
  - *The "I buy the breakout / target resistance" instruction* → dropped to
    *Rejected / no-use* (stating it would require trade-recommendation framing).
  - *Safe transformation (the teaching point)* — the only extractable items:
    - `HUMAN-REVIEW ONLY: candidate predicate — completed-bar close above the
      most recent prior swing high (observable, falsifiable; a structural
      condition, not a buy signal)`
    - `HUMAN-REVIEW ONLY: candidate replay-fixture idea — historical bars where a
      completed bar closed above a prior swing high with volume above its N-bar
      average (observation only; no outcome or edge claim)`
  - *Risks/caveats:* "Source asserts an unsupported win-rate; treat as
    marketing/speculation."
  - *Skill verdict:* **Ready for human review.**
- **Step 4 — trading-safety-review:** the note is non-advisory (edge claim and
  buy instruction already dropped) → **Pass with cautions** (caution: the dropped
  claims must stay dropped; the predicate is phrased as a condition, not a
  signal).
- **Step 5 — product-scope-review:** the predicate candidate is staged
  HUMAN-REVIEW ONLY, not wired → **In scope with constraints** (constraint: it may
  advance only via the EntryLens promotion lock and must never become a live
  scanner/alert).
- **Outcome:** safe lesson + a labeled candidate; nothing promoted without a
  separate manual decision.

---

## 9. Definition of Done

**Runbook DoD (this file):** exists at `06_YouTube-Lesson-Library/workflow.md`;
sequences the full chain including the two doctrine pre-gates; cites (does not
copy) the skills, template, and policy files; duplicates no schema; introduces no
engine code, automation, or template rewrite.

**Per-transcript-run DoD:**
- [ ] Direct-request hard stop cleared.
- [ ] Rights/IP gate run; not Red.
- [ ] source-quality = Accept for claim-audit / Accept with restrictions.
- [ ] claim-audit run on every factual/performance claim.
- [ ] trading-safety-review = Pass / Pass with cautions.
- [ ] product-scope-review = In scope / In scope with constraints.
- [ ] Zero raw transcript committed (`git status` clean of `raw-transcripts/`).
- [ ] Every EntryLens candidate line prefixed `HUMAN-REVIEW ONLY:`.
- [ ] Green, if referenced anywhere, is verbatim.
- [ ] Note lands only in `packets/` (→ `lessons/` if promoted); nothing written
      to `03_EntryLens/` except via the EntryLens promotion lock.
- [ ] Any promotion done via `promotion-rules.md` with human approval.

---

## 10. Evidence to record in a proof log after the first real run

Model the entry on `09_Proof-Logs/product-scope-review-first-live-runs.md`
(`Templates/proof-log.md` frontmatter: `id` · `date` · `linked_decision:
ADR-0001`). Record:

- Source identity — title / creator / date — **reference only, no raw
  transcript**; rights-gate color.
- source-quality verdict.
- claim-audit tallies: counts of supported / unsupported / blocked claims.
- Which advice/edge claims were correctly **dropped** (e.g. the win-rate claim,
  the buy instruction).
- trading-safety-review and product-scope-review verdicts.
- Confirmation no raw path was committed (`git status` evidence).
- Confirmation every EntryLens candidate line is labeled `HUMAN-REVIEW ONLY:`.
- Green reprinted verbatim if it was referenced.
- Destination(s) written (`packets/`, and `lessons/` if promoted).
- Verdict (usable / pass), known limitations, recommended next proof.
- Follow-up: add the matching row to `00_System/Proof-Index.md` (typically
  outside a single task's allowed-file scope — note it as deferred).

---

## Source-of-truth files

`Master-Blueprint-V1.md` §7 (learning refinery) and §1 (EntryLens status /
Green); `00_System/Rights-IP-Gate.md`; this folder's `transcript-policy.md`,
`source-quality-rules.md`, `lesson-template.md`, `promotion-rules.md`;
`Templates/youtube-ingest.md`; and the skills `.claude/skills/source-quality`,
`/claim-audit`, `/youtube-ingest`, `/trading-safety-review`,
`/product-scope-review`. ADR-0001
(`08_Decision-Logs/ADR-0001-trader-intelligence-company.md`).
