# ADR-0007 — Record: engine-level float-replay lock closed (BD-0002 resolved); PC-0005 float-lock arc complete

```yaml
id: ADR-0007
date: 2026-06-25
owner: bigmannot23
status: accepted        # proposed | accepted | superseded
proof_required: no
```

**Status:** Accepted (2026-06-25). Status is recorded in prose because
`Templates/decision-log.md` and `00_System/Decision-Index.md` carry no
`status` field — see `.claude/rules/decision-ledger-discipline.md` (known gap).

**Decision (record):**

This is an informational state record (like ADR-0003 and ADR-0006), not a new
doctrine call. It records that the engine-level float-replay lock — tracked in
AIOS as blocker **BD-0002** — is now **RESOLVED**, closing a three-gate arc
transcribed from EntryLens this session:

1. **Gate 1 — doctrine.** **EL-ADR-019** (engine-level float-replay lock) is
   **Accepted** on entrylens `main`. It pins float accumulation order,
   precision, and band-rounding for deterministic replay.

2. **Gate 2 — build + proof corpus.** The PC-0005 sigma band plus the
   **PP1–PP19** proof corpus shipped **green** via **PR #41**.

3. **Gate 3 — promotable flip.** PC-0005 `promotable` flipped **false→true**
   across all **6 spec mirrors** (**PR #42**, commit **0135dc6**). Code
   comments were synced (**PR #43**).

Net: entrylens `main` = **0e4ba03**, internally consistent and git-verified.
The hard gate that AIOS recorded as BD-0002 (and that the PC-0005 register row
named as a HARD GATE before promotion) is cleared.

**Context:**

BD-0002 was the last build-blocking gate on the PC-0005 (sigma-band) slice:
deterministic replay required the float lock before PC-0005 could be promoted.
With EL-ADR-019 Accepted and the PP1–PP19 corpus green, the lock is in place
and the promotable flag flipped — so PC-0005's gate is closed and Build-Order's
current-position section no longer lists PC-0005 as a gated candidate. The next
real candidate, PC-0004 (maturity guard), still needs a defining ADR before it
is buildable; selecting it remains a separate target-selection decision, not
decided here.

**Cross-repo caveat:**

All four facts (EL-ADR-019, PR #41, PR #42 / 0135dc6, entrylens `main` 0e4ba03)
are as of the EntryLens facts transcribed this session. An AIOS session cannot
read or re-confirm entrylens-platform directly — see
`00_System/Blockers-Debt-Ledger.md` **BD-0007**. Authority lives in EntryLens
EL-ADR-019 and the PR #41 / #42 merges, not in this repo.

**Alternatives rejected:**

- *Don't record it* — rejected: closing a BLOCKS-BUILD blocker that gated a
  predicate slice is a durable cross-repo state change worth a row.
- *Re-derive / re-confirm EntryLens state here* — rejected: an AIOS session
  can't (BD-0007); record-as-transcribed is the existing convention (ADR-0003,
  ADR-0006).
- *Declare PC-0004 the committed next target in this ADR* — rejected: target
  selection is a separate decision; this record only notes PC-0005's gate
  cleared and PC-0004 as the pending-ADR candidate.

**Rationale:**

The float-replay lock was the single hard gate on PC-0005 promotion. Recording
EL-ADR-019 and the PR #41 / #42 chain keeps the AIOS ledger honest about what
unblocked, and lets BD-0002 be marked RESOLVED and Build-Order's current
position be corrected.

**Out of scope / follow-ups (not done here):**

- The PC-0005 row in `04_AlphaLab/Predicate-Candidates/_index.md` still names
  BD-0002 as a HARD GATE before promotion; updating it is a separate change.
- BD-0008 still lacks a RESOLVED marker; untouched here by instruction.

**Proof required:** No. State record of a cross-repo lock closure; no AIOS
behavioral code change.

**Links:**

- EntryLens `EL-ADR-019` (engine-level float-replay lock, Accepted) — EntryLens
  repo, not re-confirmable from AIOS (BD-0007).
- EntryLens PR #41 (PC-0005 sigma band + PP1–PP19 proof corpus, green).
- EntryLens PR #42 / commit 0135dc6 (PC-0005 `promotable` false→true, 6 spec
  mirrors); PR #43 (code comments synced). entrylens `main` = 0e4ba03.
- `00_System/Blockers-Debt-Ledger.md` **BD-0002 (RESOLVED)**.
- `00_System/Build-Order.md` CURRENT POSITION + dependency-chain link #7
  (PC-0005 gate cleared).
