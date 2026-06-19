---
read_first: NonGoals.md
source_of_truth: Master-Blueprint-V1.md §1
---

# EntryLens — Product Brief

> **This folder is pointer/staging only.** Deterministic EntryLens engine
> code lives in a separate `entrylens/` product repo, created later. Nothing
> in this folder is runtime code, and nothing in this folder ever will be —
> when the separate repo exists, this folder stays staging/pointer only.

## What EntryLens is

A deterministic live *plan-validity meter*. Tiny frontend, large
deterministic backend. It checks whether the trader's own predeclared plan
is currently, structurally satisfied — nothing more.

## The canonical doctrine (locked, verbatim — do not paraphrase)

> **Green = every required condition in the trader's locked declared plan
> is deterministically satisfied right now.**
> **Green does not mean buy, trade, enter, high probability, edge, or best
> time.**

This wording is locked by explicit decision on 2026-06-18
(`Master-Blueprint-V1.md` §1, the single highest-authority doctrine in this
entire stack). It must appear verbatim, unparaphrased, anywhere EntryLens
status is described in product surfaces, UX copy, or marketing.

## Status semantics

| Status | Meaning |
|---|---|
| **Green** | Every required declared-plan condition is deterministically satisfied right now. Permission under the trader's own plan — not advice. |
| **Yellow** | Forming / waiting; not all conditions met. |
| **Orange** | Avoid: chase risk, wide risk, chop, late, degraded, or unclear under valid evidence. |
| **Red** | Invalidated and terminal for this active plan version. |
| **Gray** | No trustworthy judgment (stale / missing / contradictory / unsupported / vision-only evidence). Fail closed. |

All five statuses are deterministic-engine output only. No model computes
or overrides live status — see `NonGoals.md` and
`00_System/Safety-Policy.md`.

## "B setup" / "B-minimum"

A deterministic plan-completeness gate only — never an edge grade,
probability score, or AI quality judgment. A setup is "B-minimum" only if
the trader's predeclared plan contains all nine required structural
conditions (active plan, direction, setup type, trigger rule, invalidation
rule, risk gate, confirmation rule, no-trade filters, fresh/reference-ready
data). Missing or stale any one of the nine means EntryLens cannot be
Green.

## The five locks

1. **Product doctrine lock** — `entrylens/docs/entrylens-os-v2.md` (in the
   separate product repo, once it exists) defines scope; this blueprint
   defines the boundary.
2. **Runtime lock** — only deterministic engine code emits status.
3. **Language lock** — a banned trading-advice phrase scanner runs on
   outputs and UX copy.
4. **Proof lock** — runtime-sensitive changes require tests + proof log +
   fixture.
5. **Promotion lock** — AlphaLab proposals enter EntryLens only via a
   manually approved promotion packet + spec delta + tests (see
   `00_System/Promotion-Rules.md`).

## Where the real engine lives

Not here. When `entrylens/` is created (blueprint §9 build order step 7),
it holds deterministic engine code, specs, fixtures, and runtime proof
logs — and never AlphaLab raw research, personal trading logs, raw
screenshots, course libraries, or broker/account/P&L anything. This folder
remains the pointer/staging side on the AIOS-brain side of that boundary.
