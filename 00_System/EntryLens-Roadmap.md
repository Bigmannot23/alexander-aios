---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md (this is a living working view; the blueprint and the decision logs it cites win on conflict)
---

# EntryLens Roadmap

## What belongs here

A single LIVING view of where EntryLens is: what is already true (ground
truth), what is being verified next (the moving slices), and what is
deliberately parked. Two deliberate layers — a stable top layer (phases +
done-criteria, rarely changes) and a moving layer (detailed only for the next
1–3 Phase-1 slices). This document is expected to change as slices land; that
is the point.

## What does not belong here

- Decision records. Those are immutable point-in-time logs in
  `08_Decision-Logs/` with rows in `00_System/Decision-Index.md`. This roadmap
  links them; it never replaces or rewrites them.
- Trade advice or signals of any kind, or any redefinition of GREEN.
- EntryLens engine/runtime code (separate `entrylens/` repo, later).
- Detailed steps for Phase 2+ — those are scoped when reached, not invented now.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` (canonical doctrine). The decision logs this roadmap
references — `08_Decision-Logs/ADR-0001-trader-intelligence-company.md`,
`08_Decision-Logs/ADR-0002-permission-posture.md`, and
`08_Decision-Logs/DIRECTION-0001-market-context-evidence-layer.md` — win over
this living view on any conflict. If this file disagrees with a decision log,
the decision log is right and this file is stale.

---

## How to read this document

This roadmap has two layers, on purpose:

- **Stable layer (below, "Phases"):** the phase structure and each phase's
  done-criteria. This changes rarely. It is the skeleton.
- **Moving layer ("Next slices"):** detailed only for the next 1–3 Phase-1
  slices, in order. Everything Phase 2+ is intentionally left as "not yet
  specified — scope when reached." Do not invent later-phase detail here.

This is a LIVING document, distinct from the immutable decision logs in
`08_Decision-Logs/` that feed it. The logs are history; this is the current
view.

## Permanent constraint (applies to EVERY phase — never a phase to graduate out of)

EntryLens verifies whether the trader's OWN locked declared conditions are
deterministically true right now. It does not recommend, signal, scan, score,
predict, or judge whether conditions favor a trade. No probability, edge,
win-rate, or expected-return framing. No broker/account/order/P&L/payment
surfaces.

**Green = every required condition in the trader's locked declared plan is
deterministically satisfied right now. Green does not mean buy, trade, enter,
high probability, edge, or best time.**

This compliance boundary is a permanent constraint across all phases below —
Phase 4 does not relax it, and no phase "completes" it. It is the floor every
slice stands on.

## Ambition (stated explicitly, framed safely)

EntryLens's best version is **DEPTH on what it already does** — more
trader-declarable condition types, richer evidence, and provable trust that the
reported state is correct. It is **not breadth into signals, scoring, ranking,
or prediction.** Getting "better" means a trader can declare more kinds of
objective conditions and trust the verification more — never that the system
starts deciding what's worth trading. The permanent constraint above is the
guardrail that keeps depth from drifting into signal generation.

---

## Phases (stable layer — phases + done-criteria)

### Phase 0 — Ground truth (KNOWN / DONE)

Confirmed facts today (source: `DIRECTION-0001` §1/§4):

- Predicate inventory is complete (screen-evidence predicates specified).
- Coverage gaps are identified (the fixture/expansion work in Phase 1).
- VWAP definition is **not pinned** (referenced as a required anchor, but its
  canonical session anchor / reset / RTH-vs-ETH semantics are undefined).
- Exactly one supported setup, `vwap_reclaim_pullback_continuation`, is
  validated **only on synthetic Sprint-10 fixtures** plus passing unit tests —
  **synthetic, not real data.**

### Phase 1 — Verify the core is correct

Sequence: replay/observe harness → fixture expansion (boundary +
priority-collision + pullback-leg + fail-closed) → pin VWAP definition (Lane A,
its own EL-ADR) → one setup proven on REAL recorded data.

**Done when:** the engine's reported state is provably correct on real recorded
data, and a human can hand-check it.

### Phase 2 — One full path end-to-end

For the one supported setup: declare → lock → feed evidence → read state +
proof.

**Done when:** that loop runs start to finish without touching code.

### Phase 3 — Defensible

The compliance boundary is enforced in code (not just intent), proofs are
durable, and the decision records are complete.

**Done when:** a skeptical outsider could inspect it and the "not trading
advice" claim holds up.

### Phase 4 — Product-ready / depth

Additional setups; the market-context CONNECTIONS layer; interface; onboarding.
Each is its own scoped decision, **not assumed now.**

The market-context CONNECTIONS layer is already captured as a parked direction:
`08_Decision-Logs/DIRECTION-0001-market-context-evidence-layer.md` (DIRECTION
CAPTURED — candidate, gated, not approved for build). Its four hard gates, all
required before it becomes a buildable slice:

1. **Core verified** — deterministic replay harness proven; negative fixtures
   expanded; engine green on recorded packets.
2. **VWAP definition pinned** — canonical session anchor / reset / RTH-vs-ETH
   decided and recorded as an EL-ADR.
3. **One supported setup proven on REAL data** —
   `vwap_reclaim_pullback_continuation` passing on real recorded chart data via
   replay, not synthetic.
4. **§2 boundary fixes accepted** — context-predicate vocabulary locked as
   trader-declared neutral booleans.

Gates 1–3 are exactly Phase 1's done-criteria; gate 4 is a boundary acceptance.
Until all four hold, DIRECTION-0001 stays parked.

---

## Next slices (moving layer — ONLY the next 1–3 Phase-1 slices, in order)

### Slice 1 — Replay / observe harness

Dev infrastructure, **not a UI.** Replays recorded evidence packets through the
existing decision function and lets a human observe the reported state.

NOT-list (explicitly out of scope for this slice): no dashboard, no
persistence, no scoring, no recommendation, no live data-fetch, no plan-editing.

### Slice 2 — Fixture expansion

Expand fixtures to cover: boundary cases, priority-collision cases,
pullback-leg cases, and fail-closed cases.

### Slice 3 — Pin VWAP definition

Lane A; recorded as its **own EL-ADR**. Pinning the canonical VWAP definition
**blocks real-data work** (Phase 1's "proven on real data" cannot be trusted
until VWAP is unambiguous).

### Phase 2+ — not yet specified — scope when reached

Detailed steps for Phase 2 and later are deliberately omitted. Scope them when
Phase 1 is done, not before.

---

## Notes, known splits, and parked items

- **Parked Phase-4 CONNECTIONS slice:**
  `08_Decision-Logs/DIRECTION-0001-market-context-evidence-layer.md`, with its
  four gates (above). Parked until those gates hold.

- **KNOWN SPLIT to reconcile (future cleanup — do NOT reconcile now):** the
  EL-ADR series (e.g. EL-ADR-007 / 008 / 009) lives in the EntryLens product
  repo's decisions folder. This AIOS repo uses its own `ADR-` / `DIRECTION-`
  series (`00_System/Decision-Index.md`). The two series are tracked
  separately; reconciling them is a later cleanup, not done here.

- **Parked Lane C items (carried, not scheduled):**
  - `CALL` / `CALLS` string seam (naming that must not leak trade-verb
    direction into neutral predicates).
  - Python stop-hook.
  - `apps/web` + `apps/desktop` test failures — unrelated to the engine.

- **Living-document status:** This file is a LIVING document and is expected to
  change as slices land. It is distinct from the immutable decision logs in
  `08_Decision-Logs/` that feed it — those are point-in-time history and are not
  edited; this roadmap is the current view and is.
