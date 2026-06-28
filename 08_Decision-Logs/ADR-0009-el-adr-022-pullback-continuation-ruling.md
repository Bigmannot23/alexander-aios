# ADR-0009 — Record: EL-ADR-022 Accepted — VWAP_RECLAIM_PULLBACK_v1 pullback/continuation predicate gap resolved by ruling (predicates not yet built)

```yaml
id: ADR-0009
date: 2026-06-27
owner: bigmannot23
status: accepted        # proposed | accepted | superseded
proof_required: no
```

**Status:** Accepted (2026-06-27). Status is recorded in prose because
`Templates/decision-log.md` and `00_System/Decision-Index.md` carry no
`status` field — see `.claude/rules/decision-ledger-discipline.md` (known gap).

**Decision (record):**

This is an informational cross-repo state record (like ADR-0003, ADR-0006, and
ADR-0007), not a new AIOS doctrine call. It records that **EL-ADR-022**
(entrylens-platform) is **Accepted**, resolving — at the ruling level — the
`VWAP_RECLAIM_PULLBACK_v1` pullback/continuation predicate gap that
**EL-ADR-021 line 77** logged as blocking GREEN-on-PC.

The ruling, as transcribed from EntryLens this session:

1. **Two new predicates, close-only and stateless.**
   - **PC-0006 — pullback-retest.** Close-only; a retest of the reclaim level
     within a tolerance `τ` (default **0.002**, convention-unverified — the
     numeric default and its convention are not re-confirmable from an AIOS
     session).
   - **PC-0007 — continuation-resumption.** Close-only; a close **above the
     reclaim-leg high** after the pullback.

2. **Three-leg merge.** The setup composes as **PC-0002 (reclaim) + PC-0006
   (pullback) + PC-0007 (continuation)**.

3. **Anchoring = last-uninvalidated reclaim.** The pullback/continuation legs
   anchor to the most recent reclaim that has not been invalidated.

4. **Replay-lock preserved.** Both new predicates are stateless and
   re-derivable from closes, so deterministic replay is preserved; evaluation
   is **fail-closed** throughout.

**Status nuance (do not over-read this record):**

- The **RULING is Accepted.** The **predicates are NOT yet built** — PC-0006
  and PC-0007 do not exist as code in any repo. Build is a separate forthcoming
  **Lane B** sprint.
- **D4 wiring is deferred** — later still, after build.
- Nothing here is shipped, merged, or proven. This record must not be read as a
  build/proof receipt.

**Canonical Green wording (Master-Blueprint §1.1, verbatim — do not
paraphrase):**

Green = every required condition in the trader's locked declared plan is
deterministically satisfied right now. Green does not mean buy, trade, enter,
high probability, edge, or best time.

**Context:**

`EL-ADR-021` line 77 logged the pullback/continuation predicate gap as the
condition blocking GREEN on the `vwap_reclaim_pullback_continuation` setup
family (the one supported setup named in `00_System/EntryLens-Roadmap.md`).
This gap was never logged as an AIOS-side blocker (BD entry), because it did
not block on the AIOS side — so `00_System/Blockers-Debt-Ledger.md` is left
untouched by this record. EL-ADR-022 supplies the ruling that closes that
EntryLens-side gap; this ADR pins the cross-repo provenance in the AIOS ledger.

**Cross-repo caveat:**

All facts (EL-ADR-022, EL-ADR-021 line 77, the PC-0006/PC-0007 definitions, the
`τ` default, and the three-leg composition) are as transcribed from EntryLens
this session. An AIOS session cannot read or re-confirm entrylens-platform
directly — see `00_System/Blockers-Debt-Ledger.md` **BD-0007**. Authority lives
in EntryLens `EL-ADR-022`, not in this repo. This ADR adds no runtime code and
changes no engine behavior.

**Alternatives rejected:**

- *Don't record it* — rejected: an Accepted EL-ADR that resolves a
  GREEN-blocking predicate gap is a durable cross-repo state change worth a row.
- *Open a BD entry to "close" the gap on the AIOS side* — rejected: the gap
  never blocked on the AIOS side, so there is nothing to close there; inventing
  a BD entry just to mark it resolved would misrepresent the ledger.
- *Mark PC-0006 / PC-0007 as built or promotable* — rejected: only the ruling
  is Accepted; the predicates do not exist as code. Recording them as built
  would be false.
- *Re-derive / re-confirm EntryLens state here* — rejected: an AIOS session
  can't (BD-0007); record-as-transcribed is the existing convention (ADR-0003,
  ADR-0006, ADR-0007).

**Rationale:**

The pullback/continuation gap was the EntryLens-side condition holding GREEN on
the only supported setup family. Recording EL-ADR-022 keeps the AIOS ledger
honest about what the ruling resolved — while the status nuance (ruling
Accepted, predicates unbuilt, D4 wiring deferred) keeps the record from being
mistaken for a build or proof receipt.

**Out of scope / follow-ups (not done here):**

- Building PC-0006 and PC-0007 — a separate forthcoming Lane B sprint.
- D4 wiring of the three-leg merge — later still.
- The `τ` default (0.002) and its convention remain convention-unverified until
  a build sprint pins and proves them.
- No change to `00_System/Blockers-Debt-Ledger.md` (no AIOS-side blocker
  existed for this gap), Master-Blueprint, EntryLens runtime, or any
  EL-ADR-NNNN.

**Proof required:** No. State record of a cross-repo ruling; no AIOS
behavioral code change, and no AIOS-buildable artifact created.

**Links:**

- EntryLens `EL-ADR-022`
  (`docs/adr/EL-ADR-022-vwap-reclaim-pullback-continuation-predicates.md`,
  Accepted) — EntryLens repo, not re-confirmable from AIOS (BD-0007). Closes
  the gap logged at `EL-ADR-021` line 77.
- `00_System/EntryLens-Roadmap.md` (`vwap_reclaim_pullback_continuation` — the
  one supported setup family this ruling unblocks at the ruling level).
- `.claude/rules/entrylens-trading-safety.md` (Green ≠ buy; no AI-authorized
  Green; fail-closed).
- `00_System/Blockers-Debt-Ledger.md` **BD-0007** (cross-repo reads require
  manual handoff; why every EntryLens reference here is transcribed, not
  re-confirmed).
- Prior cross-repo record ADRs: ADR-0003, ADR-0006, ADR-0007.
