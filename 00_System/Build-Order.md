---
read_first: CLAUDE.md
source_of_truth: EntryLens-Roadmap.md owns phases; Master-Blueprint-V1.md §3 wins on conflict. Cross-repo (EntryLens) chain links are verified only as of the last EntryLens STATE REPORT — see Blockers-Debt-Ledger BD-0007.
---

# Build Order

The tactical "what's buildable next" layer. This file does NOT own phases or
done-criteria — those live in `00_System/EntryLens-Roadmap.md` (the phase
home). This file holds only the slice-level **dependency chain** and the
single current next-buildable position, anchored to git truth.

**Phase home:** `00_System/EntryLens-Roadmap.md` (phases, done-criteria, and
the next Phase-1 slices). Read it for *where* in the arc a link sits; read
this for *what unlocks what*.

## Status legend

- **DONE (in-repo)** — verified present in this repo this session.
- **DONE (cross-repo, verified)** — verified present on EntryLens `main` via an
  EntryLens STATE REPORT (date + commit noted). An AIOS session cannot re-confirm
  it directly (see `00_System/Blockers-Debt-Ledger.md` BD-0007); status is as of
  the cited report.
- **PENDING** — not yet built; no blocker recorded.
- **BLOCKED** — a recorded blocker (BD-xxxx) gates it.
- **UNVERIFIED (cross-repo)** — lives in EntryLens, not yet confirmed by a STATE
  REPORT.

## Dependency chain

Each link unlocks the one after it. Status is from git truth, not handoff narrative.
Cross-repo links 1, 2, 3 confirmed via EntryLens STATE REPORT @ origin/main 60a4410.

1. **VWAP line — `computeD1Vwap`** — **DONE (cross-repo, verified).**
   `packages/engine/src/vwap/d1Vwap.ts`, landed PR #16 / EL-ADR-011. Fail-closed
   (CV=0 → vwap null, never fabricated). Verified on EntryLens main 60a4410.
2. **Four-state status type — `TAPredicateStatusV1`** — **DONE (cross-repo,
   verified).** `packages/engine/src/taPredicateStatus.ts`, landed PR #17. Four-state
   vocab with fail-closed boolean projection. Verified on EntryLens main 60a4410.
3. **PC definitions — EL-ADR-014** — **DONE (cross-repo, verified).**
   `docs/adr/EL-ADR-014-…md`, landed PR #18. Formalizes PC-0001/PC-0002 with
   objective close-based rules. NOTE: EL-ADR-014 explicitly leaves PC-0002's
   consecutive-vs-cumulative counting UNRESOLVED — consistent with BD-0006. The
   AIOS-side RAW rows for PC-0001/PC-0002 are also DONE (in-repo):
   `04_AlphaLab/Predicate-Candidates/_index.md`. Verified on EntryLens main 60a4410.
4. **PC-0001 build (calls)** — **DONE (cross-repo, verified).** Calls branch shipped
   to EntryLens `main` (commit 68c28f5, merged). Status as of last EntryLens STATE
   REPORT; an AIOS session cannot re-confirm directly (see BD-0007). Puts side now DONE —
   puts `tieRule` PINNED as Set A + mirror by **EL-ADR-017** and shipped to EntryLens `main`
   (merged PR #33), resolving **BD-0008 / BD-PC0001-PUTS** (record: ADR-0006).
5. **PC-0002** — **DONE (cross-repo, verified).** N-of-M close-counting PINNED by
   EL-ADR-015 (entrylens-platform, commit 16c89fb): consecutive / anywhere-in-M /
   calls-only — `00_System/Blockers-Debt-Ledger.md` **BD-0006 (RESOLVED)**. Shipped
   (calls) to EntryLens `main` (commit 86a40c8). Status as of the EntryLens facts
   transcribed this session; an AIOS session cannot re-confirm directly (see BD-0007).
6. **PC-0003** — **DONE (cross-repo, verified).** VWAP hold-above-N shipped to EntryLens
   `main` (commit 82578cb, EL-ADR-016 Approved — Option C reset, N-only must-declare). Engine
   suite now 1085 tests / 22 files and a **required CI gate.** Status as of the EntryLens facts transcribed
   this session; an AIOS session cannot re-confirm directly (see BD-0007). AIOS-side RAW row
   reconciled to EL-ADR-016 (`04_AlphaLab/Predicate-Candidates/_index.md`).
7. **Sigma-band (PC-0005) + float-replay lock** — **DONE (cross-repo, verified).** The engine
   float-replay lock (accumulation order, precision, band-rounding) is pinned by **EL-ADR-019**
   (Accepted on entrylens `main`); PC-0005 sigma band + PP1–PP19 proof corpus shipped green
   (PR #41) and PC-0005 `promotable` flipped false→true across all 6 spec mirrors (PR #42,
   commit 0135dc6). entrylens `main` = 0e4ba03. **BD-0002 closed.** Status as of the EntryLens
   facts transcribed this session; an AIOS session cannot re-confirm directly (see BD-0007).
8. **Gate-casing before real locking** — **DONE (cross-repo, verified).** The
   Plan-Quality-Gate now accepts canonical lowercase `calls`/`puts` calls
   (implements EL-ADR-012), so `plan-declaration-v1` plans lock through the gate —
   real plan-lock now works. Shipped to EntryLens `main` (commit 47cb300); engine
   suite now 1085 tests / 22 files, behind the required CI gate. **BD-0001 closed.** Status as
   of the EntryLens facts transcribed this session; an AIOS session cannot re-confirm
   directly (see BD-0007).

## CURRENT POSITION

**Next target = UNDECIDED — pending a target-selection decision.** PC-0001, PC-0002,
and PC-0003 have all shipped to EntryLens `main` — calls earlier (PC-0001 commit 68c28f5,
PC-0002 commit 86a40c8, PC-0003 commit 82578cb, EL-ADR-016 Approved), and the **puts arc now
DONE**: puts `tieRule` PINNED as Set A + mirror by **EL-ADR-017**, PC-0001/0002/0003 puts
merged to EntryLens `main` (PR #33), resolving **BD-0008 / BD-PC0001-PUTS** (record: ADR-0006).
BD-0001 is now closed — the Plan-Quality-Gate accepts canonical lowercase `calls`/`puts`
(EL-ADR-012), so real plan-lock works (commit 47cb300; engine suite now 1085 tests / 22 files,
behind the required CI gate). The **PC-0005 float-replay gate is now closed too** — the
engine-level float-replay lock is pinned by **EL-ADR-019** (Accepted on entrylens `main`);
PC-0005 sigma band + PP1–PP19 proof corpus shipped green (PR #41), and PC-0005 `promotable`
flipped false→true across all 6 spec mirrors (PR #42, commit 0135dc6). entrylens `main` = 0e4ba03.
This clears **BD-0002**. Status on all of the above is as of the EntryLens facts
transcribed this session; an AIOS session cannot re-confirm directly (see BD-0007).

No next slice is committed without a decision — with the float-replay gate cleared, the
remaining real candidate is:

- **PC-0004 (maturity guard)** — needs a defining ADR before it can be built; next candidate
  pending that ADR.

(PC-0005 (sigma-band) is no longer gated — the engine float-replay lock **BD-0002** is closed,
see above. Puts-side / BD-0008 pinned by EL-ADR-017 and shipped.)

Picking the next target is a target-selection decision (decision log + Decision-Index), not a
Build-Order call — not decided here.

## Pointers

- Phases / done-criteria / next slices: `00_System/EntryLens-Roadmap.md`
- Blockers referenced here (BD-0001, BD-0002, BD-0006, BD-0007):
  `00_System/Blockers-Debt-Ledger.md`
- PC candidate rows: `04_AlphaLab/Predicate-Candidates/_index.md`
