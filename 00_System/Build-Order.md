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
4. **PC-0001 build (calls)** — **PENDING** (← CURRENT POSITION). All three
   prerequisites (links 1-3) are now confirmed present on EntryLens main. The build
   itself is not yet done. See CURRENT POSITION.
5. **PC-0002** — **BLOCKED.** N-of-M close-counting (consecutive vs cumulative within
   the M window) is not pinned — `00_System/Blockers-Debt-Ledger.md` **BD-0006**.
   Both the in-repo PC-0002 row and EL-ADR-014 are verified SILENT/UNRESOLVED on this.
6. **PC-0003** — **PENDING.** Row staged RAW in-repo
   (`04_AlphaLab/Predicate-Candidates/_index.md`); build cross-repo, no recorded blocker.
7. **Sigma-band (PC-0005) + float-replay lock** — **BLOCKED.** PC-0005 row staged RAW
   in-repo; promotion hard-gated on the engine float-replay lock (accumulation order,
   precision, band-rounding) — **BD-0002**.
8. **Gate-casing before real locking** — **BLOCKED.** Lowercase `calls`/`puts` plans
   can't lock through the uppercase Plan-Quality-Gate; needs a casing shim or gate
   update before the engine locks a real plan — **BD-0001** (EL-ADR-012; vocab present
   in EntryLens, gate-casing fix itself not yet done).

## CURRENT POSITION

**Next buildable = PC-0001 (calls)** — the first true end-to-end predicate
(price-to-VWAP position at bar close). Its three prerequisites are CONFIRMED present on
EntryLens main (links 1-3, verified @ 60a4410): real four-state status type, real
definition in EL-ADR-014, real VWAP line. PC-0001 is buildable now. Nothing past it is
buildable until its blocker clears (PC-0002 → BD-0006; sigma-band → BD-0002; real
locking → BD-0001).

## Pointers

- Phases / done-criteria / next slices: `00_System/EntryLens-Roadmap.md`
- Blockers referenced here (BD-0001, BD-0002, BD-0006, BD-0007):
  `00_System/Blockers-Debt-Ledger.md`
- PC candidate rows: `04_AlphaLab/Predicate-Candidates/_index.md`
