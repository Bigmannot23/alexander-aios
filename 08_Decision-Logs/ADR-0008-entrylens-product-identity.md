# ADR-0008 — Ratify EntryLens product identity (what it IS / what it is NOT), doctrine record

```yaml
id: ADR-0008
date: 2026-06-26
owner: bigmannot23
status: accepted        # proposed | accepted | superseded
proof_required: no
```
Status: Accepted (2026-06-26). Status is recorded in prose because
Templates/decision-log.md and 00_System/Decision-Index.md carry no
status field — see .claude/rules/decision-ledger-discipline.md (known gap).

## Decision (ratify):

Ratify EntryLens's product identity as a standing AIOS doctrine record. This
record decides nothing new about the engine; it pins, in the AIOS decision
ledger, the identity that 00_System/Master-Blueprint-V1.md §1 and §1.5
already lock — so the boundary is defensible from the ledger, not only from
the blueprint body.

**What EntryLens IS.** A deterministic plan-/execution-alignment support tool
for a single human discretionary trader. It checks whether the trader's own
locked, predeclared plan is structurally satisfied right now and reports a
deterministic status. The canonical status wording is fixed by Master-Blueprint
§1.1 (quoted verbatim below) and is not redefined here.

**What EntryLens is NOT (Master-Blueprint §1.5, verbatim):**

A scanner, signal generator, buy/sell recommender, high-probability setup
finder, broker execution layer, contract picker, exit manager, trading
dashboard, market predictor, generalized trading coach, journal-first
product, auto-trader, or live AI trading assistant.

**Vision / chart-reading boundary (L2 extraction-vs-judgment test).**
Where vision or chart-reading is later used (a Connections-phase capability),
it may emit only primitive observables that map 1:1 to a trader-declared
predicate input. It may never emit a setup/pattern classification or a verdict.
Any vision output that names a setup or grades quality is a judge — forbidden.
The engine-layer placement of the deterministic evaluator (L1), the vision
fact-extractor (L2), and the frontend status labels (L3) is recorded separately
in the forthcoming EntryLens EL-ADR, which cites this record.

**Canonical Green wording (Master-Blueprint §1.1, verbatim — do not
paraphrase):**

Green = every required condition in the trader's locked declared plan is
deterministically satisfied right now. Green does not mean buy, trade, enter,
high probability, edge, or best time.

**Highest-risk failure (Master-Blueprint §1.5, verbatim):**

False Green is the highest-risk product failure in the entire stack.

## Context:

EntryLens's identity is locked in Master-Blueprint §1 (status doctrine) and
§1.5 (the NOT-list). That locking lives in the blueprint warehouse. Product,
scope, and safety reviews repeatedly need a single ledger row to point at when
asserting "this is what EntryLens is and is not." This ADR creates that row.
It is an informational/ratification record in the convention of ADR-0003,
ADR-0006, and ADR-0007 — it cites locked doctrine, it does not amend it.

## Cross-repo caveat:

EntryLens runtime, engine code, and the EL-ADR-NNNN series live in the
separate EntryLens product repo and are not re-confirmable from an AIOS session
(see 00_System/Blockers-Debt-Ledger.md BD-0007). EL-ADR-007 (V4
endpoint disposition) is referenced here only as a cross-repo pointer, as AIOS
already records it (ADR-0003); its authority lives in the EntryLens repo, not
in this file. This ADR adds no runtime code and changes no engine behavior.

## Alternatives rejected:

Leave identity locked only in Master-Blueprint §1/§1.5 — rejected: reviews
need a ledger row to cite; a decision record makes the identity defensible
from 08_Decision-Logs/ without re-opening the blueprint.
Restate / paraphrase the identity in new wording — rejected: the §1.1 Green
line and the §1.5 NOT-list are locked verbatim; this record quotes them and
must not introduce a near-miss paraphrase.
Edit Master-Blueprint §1.5 to add an explicit "EntryLens IS" line — rejected:
the blueprint is canonical and is cited, not edited; the positive framing
belongs in this derived record.

## Rationale:

A single ratification row keeps product-scope and trading-safety reviews honest:
any drift toward scanner / signal / predictor / AI-trading-assistant behavior,
any redefinition of Green, or any vision output that judges a setup rather than
extracting a declared-predicate primitive, can be checked against one cited
record that points straight back to the locked §1/§1.5 source.

## Out of scope / follow-ups (not done here):

No change to Master-Blueprint, EntryLens runtime, or any EL-ADR-NNNN.
The status-field gap in Templates/decision-log.md /
00_System/Decision-Index.md remains a separate follow-up (recorded in
.claude/rules/decision-ledger-discipline.md).

Proof required: No. Doctrine ratification record citing locked
Master-Blueprint doctrine; no AIOS behavioral code change.

## Links:

00_System/Master-Blueprint-V1.md §1.1 (canonical Green wording, LOCKED) and
§1.5 (EntryLens is NOT + false-Green highest-risk-failure line).
.claude/rules/entrylens-trading-safety.md (Green ≠ buy; no AI-authorized Green).
EntryLens EL-ADR-007 (V4 endpoint disposition) — cross-repo pointer, not
re-confirmable from AIOS (BD-0007).
Prior ratification/state records: ADR-0003, ADR-0006, ADR-0007.
