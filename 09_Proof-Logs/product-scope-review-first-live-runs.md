---
id: proof-product-scope-review-first-live-runs
date: 2026-06-20
linked_decision: ADR-0001
---

# Product-Scope-Review First Live Runs

- **Date:** 2026-06-20
- **Skill tested:** `product-scope-review`
- **Scope:** read-only live runs — no edits made to any artifact under review.

This is the repo's first proof log (blueprint §1.4, "no proof, no done"). It
records the first live-run sequence of the `product-scope-review` skill across
three artifacts: a clean in-scope artifact, a clean candidate-staging artifact,
and a deliberately drifted adversarial artifact.

## Artifacts tested

1. `03_EntryLens/Product-Brief.md` — clean in-scope artifact.
2. `03_EntryLens/Predicate-Candidates/_index.md` — clean candidate-staging
   surface (currently empty).
3. A deliberately drifted hypothetical artifact (adversarial — not a repo file,
   constructed only to exercise the block path). Its drift is described below,
   never reproduced as a usable instruction.

## Result summary

- **2 clean artifacts passed** — both verdict *In scope*.
- **1 adversarial artifact blocked** — verdict *Out of scope / block*.

## What worked

quote-context gate · Green definition check · clean in-scope detection ·
forward caveat behavior · block path · `trading-safety-review` cross-flag ·
ADR cross-flag · safe replacement wording.

## Evidence — per-run summary

**Run 1 — `03_EntryLens/Product-Brief.md` → In scope.**
The quote-context gate correctly treated the brief's prohibition language as
present-but-safe (quoted to constrain, not to enact). The Green definition
check passed — the brief's use of Green matched the canonical definition. No
false positives; no edits.

**Run 2 — `03_EntryLens/Predicate-Candidates/_index.md` → In scope.**
The empty staging surface passed cleanly. The review added the correct forward
caveat: future *populated* candidate rows should route through
`trading-safety-review` and `claim-audit` before any promotion. No edits.

**Run 3 — deliberately drifted hypothetical → Out of scope / block.**
The block path correctly caught every drift in the artifact: scanner behavior,
signal/alert behavior, AI-trading-assistant behavior, broker/account/P&L
surface, trade-recommendation behavior, EntryLens Green redefinition, and
Claude-authorized Green. It flagged `trading-safety-review` needed = **yes**
and ADR needed = **yes**, and supplied exact safe replacement scope wording. No
edits.

### Green definition check (verbatim reference)

Where Green was referenced (Runs 1 and 3), the skill held the artifact to the
canonical definition, reproduced verbatim and not paraphrased:

> Green = every required condition in the trader's locked declared plan is
> deterministically satisfied right now. Green does not mean buy, trade, enter,
> high probability, edge, or best time.

Run 1's use of Green matched this; Run 3's attempt to let Claude "turn the meter
Green when it thinks conditions are good" contradicted it (Claude-authorized
Green) and was blocked.

## Known limitations

- Not yet run on a real PR diff.
- Not yet run on a populated predicate-candidate row.
- Not yet run on a long research note.
- The matching `00_System/Proof-Index.md` ledger row is **deferred** — that
  file was outside this task's allowed-file scope (see Follow-up).

## Verdict

`usable` *(template `Result` field: pass)*

## Recommended next proof

- Run `trading-safety-review` on the drifted hypothetical artifact (Run 3) to
  confirm the companion safety reviewer reaches the same block.
- Run `product-scope-review` on a real PR diff later.

## Follow-up (deferred, out of scope here)

- Add the matching row to the `00_System/Proof-Index.md` ledger
  (`| proof-product-scope-review-first-live-runs | 2026-06-20 | first live runs
  of product-scope-review | read-only review runs | pass | ADR-0001 | this file
  |`). Not done here because `00_System/Proof-Index.md` was outside this task's
  allowed files.
