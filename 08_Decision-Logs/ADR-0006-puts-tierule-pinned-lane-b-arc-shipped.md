# ADR-0006 — Record: EL-ADR-017 puts tieRule pinned; Lane B puts arc shipped (PC-0001/0002/0003)

```yaml
id: ADR-0006
date: 2026-06-24
owner: bigmannot23
status: accepted        # proposed | accepted | superseded
proof_required: no
```

**Status:** Accepted (2026-06-24). Status is recorded in prose because
`Templates/decision-log.md` and `00_System/Decision-Index.md` carry no
`status` field — see `.claude/rules/decision-ledger-discipline.md` (known gap).

**Decision (record):**

This is an informational state record (like ADR-0003), not a new doctrine
call. It records two cross-repo facts transcribed from EntryLens this session:

1. **EL-ADR-017 pins the puts `tieRule`.** A single consolidating ADR fixes
   the puts-side tie rule as **Set A + mirror**, resolving the previously-open
   blocker **BD-0008 / BD-PC0001-PUTS** (puts direction had returned
   fail-closed "unclear"; puts `tieRule` naming was unpinned). One ADR
   consolidates what was split across BD-0008 and BD-PC0001-PUTS.

2. **The Lane B puts arc shipped.** PC-0001 / PC-0002 / PC-0003 puts sides
   shipped to EntryLens `main` via merged **PR #33**. Engine suite is
   **1085 tests / 22 files**, green and behind the required CI gate.

**Context:**

Earlier records (Build-Order, AlphaLab register) had the calls sides of
PC-0001/0002/0003 shipped, with the puts side deferred and gated on the
unpinned puts `tieRule` (BD-0008). EL-ADR-017 pins that rule and the puts arc
shipped in one Lane B run. The engine count **1085 / 22** supersedes the
stale narrative numbers (919, 904) carried in older Build-Order text.

**Cross-repo caveat:**

Both facts are as of the EntryLens facts transcribed this session. An AIOS
session cannot re-confirm EntryLens `main` directly — see
`00_System/Blockers-Debt-Ledger.md` **BD-0007**. Authority lives in EntryLens
`EL-ADR-017` and the PR #33 merge, not in this repo.

**Alternatives rejected:**

- *Don't record it* — rejected: a pinned puts `tieRule` resolving a tracked
  blocker, plus a shipped arc, is a durable cross-repo state change worth a row.
- *Re-derive / re-confirm EntryLens state here* — rejected: an AIOS session
  can't (BD-0007); record-as-transcribed is the existing convention.
- *Split into multiple ADRs* — rejected: EL-ADR-017 itself is the one-ADR
  consolidation; one AIOS record mirrors it.

**Rationale:**

The puts `tieRule` was the last gate on completing the PC-0001/0002/0003 arc.
Recording EL-ADR-017 and the merge keeps the AIOS ledger honest about what
shipped and corrects the stale test count.

**Out of scope / follow-ups (not done here):**

- `00_System/Blockers-Debt-Ledger.md` BD-0008 still shows open; marking it
  RESOLVED is a follow-up (that file is outside this change's allowed scope).
- Build-Order test-count text (919/904) full correction beyond puts-done marking.

**Proof required:** No. State record of a cross-repo merge; no AIOS
behavioral code change.

**Links:**

- EntryLens `EL-ADR-017` (puts `tieRule` Set A + mirror; consolidates
  BD-0008 / BD-PC0001-PUTS) — EntryLens repo, not re-confirmable from AIOS (BD-0007).
- EntryLens PR #33 (merged) — puts arc, engine 1085/22 green.
- `04_AlphaLab/Predicate-Candidates/_index.md` (PC-0001/0002/0003 rows).
- Supersedes stale counts in `00_System/Build-Order.md`.
