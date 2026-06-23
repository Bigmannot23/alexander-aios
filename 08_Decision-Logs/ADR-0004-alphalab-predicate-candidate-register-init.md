# ADR-0004 — AlphaLab Predicate-Candidate Register Initialized

```yaml
id: ADR-0004
date: 2026-06-23
owner: bigmannot23
status: accepted        # proposed | accepted | superseded
proof_required: no
```

**Status:** Accepted (2026-06-23). This entry records a structural change
already merged to `main`, and accepts the register's ID scheme. Status is
recorded in prose because `Templates/decision-log.md` and
`00_System/Decision-Index.md` carry no `status` field — see
`.claude/rules/decision-ledger-discipline.md` (known gap).

**Decision:**

The AlphaLab predicate-candidate register
(`04_AlphaLab/Predicate-Candidates/_index.md`) was initialized from empty with
five RAW VWAP candidates, and the register's ID scheme is fixed as
**PC-NNNN** (4-digit, mirroring the ADR-NNNN convention).

**What landed:**

- **PC-0001** — position-at-close.
- **PC-0002** — reclaim (finite lookback, close-based).
- **PC-0003** — hold-above-N (explicit wick rule).
- **PC-0004** — maturity GUARD.
- **PC-0005** — sigma-band reach.

All five are **RAW** (staging) and doctrine-passed. PC-0005's sigma + line are
locked by **EL-ADR-010 / EL-ADR-011**, with an engine-level float-replay lock
as a **HARD GATE** before any promotion.

**Provenance:**

Generic public VWAP definition (research extraction). Landed on `main` in
commit `a7bd8dd`, via PR #29.

> Scope note: `a7bd8dd` and PR #29 are this Alexander-AIOS repo. The EL-ADR-010 /
> EL-ADR-011 references pertain to the **separate EntryLens product repo** and
> are recorded as reported, not re-verified here.

**Context:**

Initializing the register was a durable structural change to AlphaLab — it
created the staging surface for predicate candidates and set the PC-NNNN naming
convention. It merged (PR #29) without a decision-log entry. Per
`.claude/rules/decision-ledger-discipline.md`, a change to a system boundary is
logged, not left in git history alone. This entry is that record.

**Candidate status (safety):**

The five candidates are **RAW / staging only** — not promoted, not approved, and
**not wired to any engine**. They are human-review candidate material, consistent
with ADR-0001 (Claude proposes candidates; it does not author, promote, or
authorize them). Promotion of PC-0005 specifically is blocked behind the
EntryLens engine-level float-replay lock named above. No trade advice, no Green
authorization, and no model-computed Green is implied by this register.

**Alternatives rejected:**

- *Leave the initialization in git history only* — rejected. A structural change
  and a new ID scheme are durable decisions; chat/commit history is not the
  ledger (see `.claude/rules/alexander-aios-boundary.md`).
- *Use ad-hoc candidate IDs* — rejected. PC-NNNN mirrors ADR-NNNN, keeping the
  register searchable and consistent with the rest of the repo.

**Rationale:**

A fixed, mirror-of-ADR ID scheme plus a RAW staging gate keeps candidate work
auditable and firmly on the safe side of the trading boundary: candidates
accumulate as reviewable proposals, promotion is gated, and nothing is wired in
by recording it here.

**Proof required** (link to `09_Proof-Logs/_index.md` entry once available):
No. This is a record of a documentation/structural change, not a behavioral
change that needs evidence of working. The provenance commit `a7bd8dd` is
git-verifiable on `main`.
