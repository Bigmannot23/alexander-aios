---
title: Operating Doctrine
status: digest
relationship_to_blueprint: >
  This is a consolidated digest of every binding "Doctrine" item in
  00_System/Master-Blueprint-V1.md, for fast operator reference. It is not
  the source of truth — the blueprint is. If this file and the blueprint
  ever disagree, the blueprint wins and this file is stale; fix this file.
---

# Operating Doctrine

Quick-reference list of binding rules. Each links to its full reasoning in
`00_System/Master-Blueprint-V1.md`. Changing any "Doctrine" item requires a
decision log (`08_Decision-Logs/_index.md`) + proof gate
(`09_Proof-Logs/_index.md`).

## 1. EntryLens status doctrine (highest authority — blueprint §1)

> **Green = every required condition in the trader's locked declared plan is
> deterministically satisfied right now. Green does not mean buy, trade,
> enter, high probability, edge, or best time.**

- "B setup" / "B-minimum" is a deterministic plan-completeness gate only —
  never an edge grade, probability score, or AI quality judgment. The nine
  required conditions are listed in blueprint §1.2.
- Only deterministic engine code emits live status. No model computes or
  overrides it.
- A banned trading-advice phrase scanner runs on outputs and UX copy.
- Runtime-sensitive changes require tests + proof log + fixture.
- AlphaLab proposals enter EntryLens only via a manually approved promotion
  packet + spec delta + tests.
- **False Green is the highest-risk product failure in this entire stack.**

## 2. Source-of-truth hierarchy (blueprint §3)

Latest explicit chat instruction → Maxed ClaudeOps Operator Blueprint →
Ultimate Claude Code OS Stack → AlphaLab Live Vision Desk Playbook →
EntryLens Second Brain Addendum → Automation Leverage Blueprint → Advanced
Power Stack → Claude Code OS for EntryLens → Nate Herk transcript
(non-binding) → older research. Conflicts resolve by scope, not recency
alone. Unresolved conflicts become a decision log, not a chat decision.

## 3. Trading safety (blueprint §6)

Claude **may**: research, summarize, audit, log, test, explain, educate,
extract deterministic rules, design non-advisory tool logic, review
mistakes, build checklists, design tests, improve execution discipline.

Claude **must not**: recommend trades/symbols/calls/puts/entries/exits/
contracts/strikes/expirations/sizing/stops/targets; claim edge, probability,
expected return, or win rate; connect to brokers, order tickets, account
balances, P&L, banking, payment, or execution surfaces; authorize EntryLens
Green; compute or override EntryLens live status.

Live Vision Desk is post-session-only for a long time. When live, it
observes/warns/logs only, in a discipline-mirror voice — never an oracle
voice (blueprint §6.2, §6.5).

## 4. Learning refinery (blueprint §7)

Hierarchy — do not collapse: raw material → private transcript/source →
lesson → promotion candidate → accepted delta. Raw material is never the
brain. Every source gets an IP/rights gate (green/amber/red) and a
source-quality score (0–20) before promotion. Every factual/legal/financial/
performance claim gets a claim-audit. Raw trading claims are transformed,
never adopted verbatim (blueprint §7.4 table).

## 5. Automation ladder (blueprint §12, §15)

`manual prompt → reusable skill → proof/log output → repeated success →
script → script-backed hook → read-only headless/Routine → supervised write
automation → productized workflow`

- No hook before 5–10 clean manual script runs.
- No routine before proof/log behavior is trusted.
- No write automation before deterministic scans exist.
- **No live trading automation, ever. No unattended trading-related
  decisioning, ever.**
- Current ceiling: manual prompts only. See
  `01_ClaudeOps/Hooks/README.md` and `01_ClaudeOps/Routines/README.md`.

## 6. Repo boundary (blueprint §4)

This monorepo is the brain. EntryLens deterministic engine code lives in a
separate `entrylens/` repo (created later). AlphaLab research enters
EntryLens *only* as a deterministic predicate/fixture/spec-delta via a
manually approved promotion packet — never directly.

## 7. Do-not-build-yet (blueprint §15)

Routines/scheduled tasks/Dispatch; hooks before scripts; EntryLens
engine/runtime code; Live Vision Desk live commentary; TradingView/browser
connections; broker-or-account MCP (ever) or any MCP before governance
exists; a subagent swarm beyond 4 read-only specialists; dashboards as a
build project; a ClaudeOps template repo extracted before `01_ClaudeOps/`
is proven; any "wake up to finished trading work" automation.

## 8. Protected absolutely

API keys, broker credentials, account/P&L data, payment/banking/email
surfaces. See `00_System/Safety-Policy.md` and the root `.gitignore`.
