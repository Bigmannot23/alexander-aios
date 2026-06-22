# DIRECTION-0001 — Market-Context Evidence Layer

```yaml
id: DIRECTION-0001
date: 2026-06-22
owner: bigmannot23
status: DIRECTION CAPTURED   # candidate — not approved for build
proof_required: no
```

**Status:** DIRECTION CAPTURED — candidate, not approved for build. Recorded in
prose because `Templates/decision-log.md` / `00_System/Decision-Index.md` carry
no `status` field yet (known gap, `.claude/rules/decision-ledger-discipline.md`).
First entry in the new DIRECTION- series.

---

Title: Two-evidence-type model (screen evidence + trader-declared market-context predicates)
Status: DIRECTION CAPTURED — candidate, not settled. Not approved for build. Not a sprint.
Roadmap layer: CONNECTIONS (live external data)
Converts to: EL-ADR candidate (assign next EL-ADR id when logged)
Owner: Alexander
Date: 2026-06-22
Belongs in: Alexander-AIOS (decision/roadmap). Not the EntryLens product repo. No product code is implied by this record.

Boundary (enforced throughout): EntryLens verifies whether the trader's OWN locked declared conditions are deterministically true right now. GREEN = every required declared condition is satisfied. GREEN does not mean buy, enter, valid entry, good scalp, or "conditions favor a trade." EntryLens does not recommend, signal, scan, or judge whether conditions favor a trade.

## 1. What's already built vs. what's new

Already built / specified (reuse, do not rebuild):

| Piece | Where it lives today |
|---|---|
| Screen-evidence half (chart predicates: price/VWAP/levels, with evidence_required / evidence_observed) | docs/ta-predicate-v1.md |
| Merge → status engine + status-bar state machine | docs/status-bar-state-machine-v1.md, docs/live-entry-check-v1.md |
| Fail-closed / degrade principle ("EL-ADR-008") | Encoded concretely in live-entry-check-v1.md §9, ta-predicate-v1.md §9, and the status-bar "Fail-Closed Behavior" section |
| Source / provenance tagging seam | Predicate provenance.source_surface + provenance.levels_source (→ trader_declared / chart_read / synthetic_fixture); bundle source_context.surface_class |
| One declared setup family | vwap_reclaim_pullback_continuation — validated only on synthetic Sprint-10 fixtures + passing unit tests (test_ta_predicate_evaluator.py). Synthetic, not real data. |

Genuinely new (the only new build surface): a background market-context service that pulls SPY / VIX / SOXX / NVDA via a market-data feed, computes trader-declared context predicates, tags them source = "market_data", and hands them to the existing merge engine. Everything else — predicate shape, merge, state model, fail-closed, source tag — already exists.

Grounding note (non-blocking): the fail-closed principle is concretely documented in the named specs above. The "EL-ADR-008" label should be confirmed against the EntryLens repo's decisions folder — the AIOS decision ledger (00_System/Decision-Index.md) currently lists only ADR-0001 and ADR-0002, so the EL-ADR series is tracked separately from AIOS's own ADRs. This doesn't change the direction; it's a citation-hygiene item for when this is logged.

## 2. Two boundary fixes — required before any of it is buildable

### 2a. Context predicates must be objective, trader-declared, observable booleans

The service must verify the trader's own declared conditions, never decide that a symbol is supportive, leading, or favorable. "Supportive / leading / for calls" are directional trade judgments — the system editorializing about the market. Kill them.

| Crosses the boundary (kill) | Restated as a neutral declarable predicate |
|---|---|
| vixSupportiveForCalls | "VIX 3-min slope < 0" (trader-declared boolean) |
| isSupportive (SPY) | "SPY > session VWAP" or "SPY last 3 candles not making lower lows" |
| isLeading / "SOXX confirming" | "SOXX > session VWAP" (or whatever boolean the trader locks) |
| "NVDA leading for calls" | "NVDA > prior-day high" or "NVDA > session VWAP" |

Rule: the service computes booleans the trader pre-declared and locked. It never labels a symbol supportive / leading / confirming / favorable, never ranks symbols, and never reports anything the trader didn't declare. Context predicates carry the same provenance / freshness / evidence_quality_band discipline as screen predicates, tagged source = "market_data".

Watch item (existing code): the screen layer already ships supports_direction: calls and describes vwap_reclaim_confirmed as "a calls-oriented VWAP reclaim condition." That is tolerable today only because direction comes from the trader's locked plan (declared_direction: "calls"), not from the engine deciding the market favors calls. It is also the exact phrasing that becomes a breach the moment it's copied into the context layer. The context predicates must not inherit "for-calls / supportive" naming. Consider normalizing the screen layer's language to "satisfies the trader's declared directional condition" in a later pass.

### 2b. Naming and messaging must be condition-language, not entry-endorsement

| Crosses the boundary (kill) | Restated in condition-language |
|---|---|
| Setup name FAILED_BREAKDOWN_RECLAIM_CALL | Name by structure, not by trade verb: e.g. reclaim_after_failed_breakdown. Direction stays a trader-declared field on the plan — not baked into the name as CALL/BUY. (vwap_reclaim_pullback_continuation is already correctly neutral.) |
| "Your declared reclaim scalp is valid now" | "All declared conditions for your locked plan are currently met." |
| "Setup is on / valid / go" | "Declared conditions met: [list of conditions]." |
| any "valid entry" / "good scalp" / "trade is on" | report condition-satisfaction, never trade validity |

The status reports that declared conditions are met — not that a trade is valid, on, or advisable.

## 3. The architecture, restated clean

Two evidence types, one engine:

Screen evidence (source = "screen"): the chart the trader is watching — price, candles, VWAP, OR levels, demand zones.
Market-context evidence (source = "market_data"): a background service reading SPY / VIX / SOXX / NVDA and computing the trader's declared context booleans so the trader isn't manually watching every symbol.

Both produce the same predicate shape, each carrying its source tag, provenance, freshness_ms, and evidence_quality_band. The existing engine merges them into the five status states GRAY / YELLOW / GREEN / ORANGE / RED. No new state vocabulary, no new authority, no second engine.

GREEN only when every required declared condition — screen and context — is deterministically true at sufficient evidence quality.

Missing / stale / weak context degrades to YELLOW. It never fakes GREEN. This is the existing fail-closed/degrade rule ("EL-ADR-008") applied to the new source — missing context is an evidence-quality condition, not engine logic.

Determinism (consistent with the core): all non-determinism introduced by a live feed is handled as evidence freshness/quality, using the machinery that already exists (freshness_ms, evidence_quality_band, provenance_present, blocker_codes) — never as engine branching. The engine's decision function stays pure and replayable: given a frozen evidence packet, the output is deterministic. The live feed sits upstream of the packet; replay runs against recorded packets, never live calls. The deterministic core is unchanged.

## 4. Where it sits in the roadmap

This is a CONNECTIONS-layer feature (live external data). Per Context → Connections → Capabilities → Cadence, it comes after the core is verified. It is not the next slice.

Hard gates — all must be true before this becomes a buildable slice:

1. Core verified. Deterministic replay harness proven; negative fixtures expanded; engine green on recorded packets. (Today: only synthetic Sprint-10 fixtures + unit tests exist — insufficient.)
2. VWAP definition pinned. Canonical session anchor / reset / RTH-vs-ETH decided and recorded as an EL-ADR. (Today: vwap is referenced as a required anchor but its definition is not pinned.)
3. One supported setup proven on REAL data. vwap_reclaim_pullback_continuation passing on real recorded chart data via replay — not synthetic. (Today: synthetic-only.)
4. §2 boundary fixes accepted and the context-predicate vocabulary locked as trader-declared neutral booleans.

Flip-to-buildable condition: when gates 1–4 hold, scope the market-context service as its own CONNECTIONS sprint with a dedicated EL-ADR covering provider choice and replay/determinism bounding. Until then this stays a captured direction.

## 5. Open questions to carry (do not resolve now)

VIX feed access + fallback. VIX index vs. tradable proxy (VIXY / VXX) — which is declarable and observable to the trader, and how the fallback is chosen without the system "picking" for them.
Data-provider choice. Deferred — needs a dedicated research pass later, not now.
Bounding live-feed non-determinism for replay. Packet recording cadence, snapshot/freeze semantics, and per-timeframe freshness thresholds (threshold values are already deferred to implementation in the existing specs).
Per-plan-declared vs. shared context library. Are context conditions declared inside each trader's locked plan, or drawn from a shared menu the trader opts into and locks? (Boundary-critical — see flag #1 below.)

## Residual smell check (doctrine lens, after the §2 fixes)

Verdict: the direction is boundary-safe to capture, and safe to build later only if the §2 fixes hold and the §4 gates are met. Specific smells to keep watching:

1. A "shared context library" with any recommended/default conditions is a scanner in disguise. If a shared menu exists, it must remain an opt-in list of neutral declarable booleans the trader chooses and locks — never a default set, never a "suggested context," or it becomes a signal generator. This is the highest-risk item.
2. Evaluating symbols the trader isn't charting (VIX/SOXX/NVDA) is structurally close to scanning. It stays inside the boundary only because (a) the trader pre-declared each condition as a boolean in their locked plan, and (b) the service verifies those declared booleans and reports nothing else. The instant it surfaces an undeclared symbol/condition, ranks symbols, or reports "what's moving," it is a scanner — reject.
3. Existing supports_direction: calls / "calls-oriented" naming must not propagate into context predicates as "supportive / for-calls."
4. Any context message reading as "conditions favor / support a trade" is a breach. Condition-language only.
5. "Missing context → YELLOW" must have no exception path that yields GREEN. Verify no "assume supportive if the feed is down" shortcut can ever be introduced (fail-closed is sacred).

This is a stored direction, not a build. Route it into AIOS as a roadmap entry / EL-ADR candidate. No product code, no sprint, until the §4 gates are cleared.

---

**EL-ADR split note:** The EL-ADR series (incl. EL-ADR-008/009) referenced in
this record is tracked in the EntryLens repo's decisions folder, separate from
AIOS's own ADR / DIRECTION series. Known split to reconcile later — not
reconciled here.
