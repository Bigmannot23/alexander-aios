# ADR-0010 — Record: EntryLens engine state — NYSE calendar 2022–2028, five-color status bundle proven + golden-locked, EL-ADR-025 hardening merged (EL-ADR-024 P2 precondition cleared)

```yaml
id: ADR-0010
date: 2026-07-01
owner: bigmannot23
status: accepted        # proposed | accepted | superseded
proof_required: no
```

**Status:** Accepted (2026-07-01). Status recorded in prose because
`Templates/decision-log.md` and `00_System/Decision-Index.md` carry no
`status` field — see `.claude/rules/decision-ledger-discipline.md` (known gap).

**Decision (record):**

Informational cross-repo state record (like ADR-0003, ADR-0006, ADR-0007,
ADR-0009), not a new AIOS doctrine call. It records four EntryLens engine-state
facts, transcribed from EntryLens this session, that AIOS had not yet recorded:

1. **NYSE session calendar extended to contiguous 2022–2028.**
2. **Five-color status bundle proven end-to-end AND golden-locked.** All five
   status colors — GRAY / YELLOW / GREEN / ORANGE / RED — are proven end-to-end
   and golden-locked (golden CORPUS = 23). The five-states milestone is DONE.
   NOTE: this is the merged **bundle** status (the five colors), distinct from
   the per-predicate four-state type `TAPredicateStatusV1` recorded at
   `00_System/Build-Order.md:37–39` — the two are different objects and neither
   supersedes the other.
3. **EL-ADR-025 (engine hardening) MERGED.** Rulings: absent-wholeness → GRAY at
   the GREEN boundary; opening-edge → GRAY. This **closes EL-ADR-024's P2
   "forthcoming Engine Hardening ADR" precondition.**
4. **f16 is intentionally fixture-covered-only (documented) — NOT a gap.**

**What this record does NOT cover (see BD-0011):** the r01 real-data fixture
(blocked-by-design) and the MarketSnapshot producer gates (P1 ✅ / P2 ✅ /
P3 ❌). Those are a blocker/gate register item, tracked in
`00_System/Blockers-Debt-Ledger.md` **BD-0011**, not here.

**Status nuance (do not over-read this record):**

- Facts 1–4 are DONE / MERGED as transcribed. Fact 3's P2-precondition clearance
  is a precondition for the MarketSnapshot producer, whose remaining gate P3
  (written licensing) is **still open** — see BD-0011. Nothing here authorizes
  the producer, real-data work, or any live-data slice.
- This record adds no runtime code and changes no engine behavior.

**Green semantics:** see `.claude/rules/entrylens-trading-safety.md` (authority;
not restated here).

**Cross-repo caveat:**

All facts (the calendar range, the five-color proof + golden CORPUS=23, EL-ADR-024,
EL-ADR-025, f16) are as transcribed from EntryLens this session. An AIOS session
cannot read or re-confirm entrylens-platform directly — see
`00_System/Blockers-Debt-Ledger.md` **BD-0007**. Authority lives in EntryLens,
not in this repo.

**Alternatives rejected:**

- *Record these in Build-Order / Blockers instead* — rejected: Build-Order is the
  PC dependency chain (`:8–11`) and these are engine milestones, not PC links;
  the Blockers ledger is for blockers, and these are completed milestones that
  never blocked an AIOS step. A cross-repo state ADR is the established home
  (ADR-0003/0006/0007/0009).
- *Overwrite Build-Order.md:37 "four-state"* — rejected: that line correctly
  describes the per-predicate type `TAPredicateStatusV1`; it is not the five-color
  bundle. Fact 2 notes the distinction rather than changing that line.

**Rationale:**

Keeps the AIOS ledger honest about EntryLens state without pretending an AIOS
session verified it and without distorting Build-Order/Blockers charters.

**Proof required:** No. Cross-repo state record; no AIOS behavioral code change.

**Links:**

- EntryLens `EL-ADR-024` (MarketSnapshot producer gates) and `EL-ADR-025`
  (engine hardening) — EntryLens repo, not re-confirmable from AIOS (BD-0007).
- `00_System/Blockers-Debt-Ledger.md` **BD-0011** (r01 blocked-by-design;
  producer P3 licensing open), **BD-0007** (cross-repo handoff).
- `00_System/Build-Order.md:37–39` (per-predicate four-state type — distinct
  from the five-color bundle in Fact 2).
- `.claude/rules/entrylens-trading-safety.md` (Green semantics authority;
  Green ≠ buy; fail-closed).
- Prior cross-repo record ADRs: ADR-0003, ADR-0006, ADR-0007, ADR-0009.
