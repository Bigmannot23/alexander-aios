---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §3 (source-of-truth hierarchy). Cross-repo (EntryLens) items are UNVERIFIED from an AIOS session — see BD-0007.
---

# Blockers & Debt Ledger

A single cross-repo register of known blockers, debt, and process constraints spanning Alexander-AIOS and the separate (not-readable-from-here) EntryLens-platform repo; every EntryLens reference is recorded as stated and tagged UNVERIFIED, because an AIOS session cannot read that repo (see BD-0007).

**Tags:** **BLOCKS-BUILD** (stops a real build step) · **COSMETIC** (stale text/rows, no build impact) · **STRUCTURAL** (architecture/vocabulary debt) · **PROCESS** (working-method constraint).

**Verification legend:** **VERIFIED (in-repo)** = checked in this repo this session · **UNRESOLVED** = expected in this repo but not found · **UNVERIFIED (cross-repo — confirm in entrylens-platform)** = lives in the other repo, not checkable from an AIOS session.

---

## BD-0001 — Gate-casing fix — **RESOLVED**

- **Resolution:** Fixed in entrylens-platform — `normalizeDirection.ts` shim maps lowercase calls/puts → CALL/PUT; gate accepts and locks to identical hash. Proven by `bd-0001-direction-normalization.test.ts`. Build-order closed at commit 7ab9b77. Verified cross-repo by read-only EntryLens evidence pass this session.
- **Tag:** BLOCKS-BUILD — now cleared
- **Repo:** EntryLens-platform
- **What it is:** Lowercase `calls`|`puts` plans can't lock through the current uppercase `CALL`/`PUT` Plan-Quality-Gate; needs a casing shim or a gate update.
- **What it blocks:** The engine locking a real plan.
- **Reference:** EL-ADR-012 — **UNVERIFIED (cross-repo — confirm in entrylens-platform).** EL-ADR-012 does not appear anywhere in this repo.

## BD-0002 — Engine float-replay lock — **RESOLVED**

- **Resolution:** Locked by EL-ADR-019 (engine-level float-replay lock, Accepted on entrylens `main`). PC-0005 sigma band + PP1–PP19 proof corpus shipped green (PR #41); PC-0005 `promotable` flipped false→true across all 6 spec mirrors (PR #42, commit 0135dc6); code comments synced (PR #43). entrylens `main` = 0e4ba03, internally consistent, git-verified cross-repo this session.
- **Tag:** BLOCKS-BUILD (scoped) — now cleared
- **Repo:** EntryLens-platform
- **What it is:** Float accumulation order, precision, and band-rounding must be locked for deterministic replay.
- **What it blocks:** PC-0005 promotion. (The PC-0005 row in `04_AlphaLab/Predicate-Candidates/_index.md` already names this as a HARD GATE before promotion.)
- **Reference:** EL-ADR-010 / EL-ADR-011 — **UNVERIFIED (cross-repo — confirm in entrylens-platform).** AIOS cites EL-ADR-010/011 in the PC-0005 row and in ADR-0004, but the EL-ADRs themselves live in EntryLens and are not verifiable here.

## BD-0003 — Five-vocabulary reconciliation

- **Tag:** STRUCTURAL
- **Repo:** EntryLens-platform
- **What it is:** Overlapping vocabularies — EntryPlan (x2), PlanDslV1, TradeIdeaContractV1, TAPredicateV1, PC-000x — need reconciliation. Deferred, reportedly homed in EL-ADR-012.
- **What it blocks:** Nothing today (deferred debt). Re-tag BLOCKS-BUILD if EL-ADR-012 turns out to gate locking; as described here it does not.
- **Reference:** EL-ADR-012 — **UNVERIFIED (cross-repo — confirm in entrylens-platform).** EL-ADR-012 does not appear anywhere in this repo.

## BD-0004 — PC-0003 / PC-0005 register-row params

- **Tag:** COSMETIC
- **Repo:** Cross-repo (rows live in AIOS `04_AlphaLab/Predicate-Candidates/_index.md`; the supersede claim lives in EntryLens).
- **What it is:** PC-0005 and PC-0003 rows reportedly still show old params (PC-0005 `timeframe`, PC-0003 `interval`) said to be superseded by EL-ADR-013.
- **In-repo check (this session):** **VERIFIED** — the live PC-0005 row lists "Must-declare: k + side + condition type + **timeframe**"; the live PC-0003 row lists "Must-declare: D1 config + N + **fixed interval** + explicit wick rule (default closes-only)." Both params are still present as stated.
- **What it blocks:** Nothing (record/display accuracy only). Tracked here, NOT edited this sprint.
- **Reference:** EL-ADR-013 supersede claim — **UNVERIFIED (cross-repo — confirm in entrylens-platform).** EL-ADR-013 does not appear in this repo.

## BD-0005 — Stale indices

- **Tag:** COSMETIC
- **Repo:** Both repos
- **What it is:** Indices lagging behind reality in two places.
- **EntryLens side:** `docs/audits/V1-ENGINE-INTAKE-AUDIT.md` §i reportedly lists only EL-ADR-001–006 while 007–014 now exist — **UNVERIFIED (cross-repo — confirm in entrylens-platform).**
- **AIOS side (this session):** **VERIFIED stale** — `08_Decision-Logs/_index.md` Status prose reads verbatim "One decision logged." while `00_System/Decision-Index.md` already ledgers five entries (ADR-0001, ADR-0002, ADR-0003, ADR-0004, DIRECTION-0001).
- **What it blocks:** Nothing (record accuracy only). Tracked here, NOT fixed this sprint.

## BD-0006 — PC-0002 N-of-M close-counting semantics — **RESOLVED**

- **Resolution:** Pinned by EL-ADR-015 (entrylens-platform, commit 16c89fb): N-of-M close-counting is **consecutive / anywhere-in-M / calls-only**.
- **Tag:** BLOCKS-BUILD (scoped) — now cleared
- **Repo:** Cross-repo (PC-0002 row in AIOS; EL-ADR-014 authoring in EntryLens).
- **What it is:** Whether PC-0002 reclaim N-of-M close-counting is consecutive vs cumulative within the M window is not pinned.
- **What it blocks:** Building PC-0002.
- **In-repo check — what the PC-0002 row actually says (this session):** The live PC-0002 row reads: "RAW: VWAP reclaim — a below->above close transition within a FINITE declared lookback, close-based. Must-declare: D1 config + finite lookback + close-based + N/M." It lists "N/M" as a must-declare field and pins "close-based," but it is **SILENT on consecutive vs cumulative** counting within the M window. The row does NOT pin this.
- **Reference:** EL-ADR-014 (claimed commit bf01c1c, PR #18) reportedly defines PC-0001 and PC-0002 but leaves this UNRESOLVED; it cited the AIOS register as staging-registry-of-record but did NOT read it, composing from the doctrine-passed extraction instead — all **UNVERIFIED (cross-repo — confirm in entrylens-platform).** EL-ADR-014, bf01c1c, and PR #18 do not appear in this repo.

## BD-0008 — PC-0001 puts seam unimplemented

- **Tag:** BLOCKS-BUILD (scoped)
- **Repo:** Both repos
- **What it is:** PC-0001 calls shipped to EntryLens `main` (commit 68c28f5, merged), but the puts side is not implemented. The PC-0001 puts direction returns fail-closed "unclear," and the puts-side `tieRule` option naming (`at_or_below` vs `strictly_below`?) is not pinned by EL-ADR-014; the canonical form belongs in the AIOS register. Code seam ref: `pc0001PositionAtClose.ts` comment "BD-PC0001-PUTS".
- **What it blocks:** PC-0001 puts completion.
- **Reference:** EL-ADR-014 (PC-0001 calls @ 68c28f5) — **UNVERIFIED (cross-repo — confirm in entrylens-platform).** The puts `tieRule` naming is unresolved there; canonical form to be pinned in the AIOS register.

## BD-0007 — Cross-repo reads require manual handoff

- **Tag:** PROCESS
- **Repo:** Both repos
- **What it is:** A Claude Code session is single-repo scoped — it cannot read the other repo's files (hit twice: PC-0001 build, and EL-ADR-014 authoring). Cross-repo tasks must be scoped so the SOURCE repo session reads and emits the needed content; never "read the other repo mid-build."
- **What it blocks:** Nothing directly — a learned working constraint with no repo reference to verify.
- **Note:** This item is the reason every EntryLens reference in BD-0001, BD-0002, BD-0003, BD-0005, and BD-0006 is tagged UNVERIFIED from an AIOS session.
