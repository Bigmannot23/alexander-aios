---
read_first: CLAUDE.md (this folder's own router)
source_of_truth: Master-Blueprint-V1.md §2, §6, §7
---

# AlphaLab — Product Brief

## What AlphaLab is

Bounded trading research, review, and mistake-mining. Creative but
**non-executing** — it compounds via files (lessons, packets, mistake
taxonomies), never via live trade decisions. Markdown knowledge work only;
no executing code lives in this folder.

## What it does

- Reviews past sessions for repeated mistakes (post-session only).
- Mines mistakes into a taxonomy with proposed *process controls* — never
  advice.
- Synthesizes setup *concepts* and no-trade *filters* as observable,
  falsifiable conditions — never as recommendations.
- Stages deterministic predicate candidates that may, after a manually
  approved promotion packet, eventually inform EntryLens (see
  `00_System/Promotion-Rules.md`). AlphaLab never writes to EntryLens
  directly.
- Runs replay drills and review rubrics to build discipline, not edge
  claims.

## What it does not do

Recommend trades, symbols, contracts, sizing, or timing. Claim edge,
probability, or win rate. Connect to brokers, accounts, or live market
data. Authorize or compute EntryLens status. See `CLAUDE.md` (this
folder's router) for the full boundary.

## Relationship to Live Vision Desk

The Live Vision Desk (a separate, not-yet-built system) is the *live*,
supervised cognitive layer that observes a TradingView-only browser
profile. AlphaLab is its post-session counterpart: where mistakes get
mined and rules get refined *after* the session, on files, with no time
pressure and no live money implications in the analysis itself.

## Relationship to EntryLens

One-way, gated bridge only. See `03_EntryLens/Predicate-Candidates/_index.md`
and `04_AlphaLab/Predicate-Candidates/_index.md` — AlphaLab proposes,
EntryLens's promotion lock decides, and only a human approval + spec delta
+ tests lets anything cross.
