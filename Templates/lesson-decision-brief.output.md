---
brief_id: LDB-<YYYYMMDD>-<nn>
date: <YYYY-MM-DD>
reviewer: Alexander Minnick
source_lesson: <path / source_id of the lesson packet>
source_rights_status: <green | amber | red>
status: draft
approved_by:
approved_date:
resulting_adr:
resulting_proof_log:
---

# Lesson Decision Brief — <source short name>

> Filled by hand using `01_ClaudeOps/Runbooks/lesson-decision-brief.md`.
> One run per lesson packet. Leave `approved_by` blank until a human signs.

## 0. Input integrity (gate — fail closed)

- lesson packet present: yes/no
- required front-matter complete: yes/no (list missing)
- claim-audit status: done | pending | n/a
- source-quality score: <value> | missing
- rights_status: green | amber | red

RULE: if rights_status=red OR fields missing → allowed buckets are ONLY (7) or
(8). Classify, stop.

## 1. Safety + scope gate

- trading material in lesson? yes/no
  - if yes: candidate predicate/setup/no-trade/replay = HUMAN-REVIEW ONLY, not
    the approved build move, routes to a Promotion-candidate FLAG, never an
    implementation packet.
  - advice/signal/probability/win-rate/edge/sizing/contract/entry/exit/broker/
    account/P&L → bucket (8).
- product-scope: any candidate that adds EntryLens runtime in AIOS, makes
  EntryLens a scanner/signal/dashboard/broker/assistant, adds a frontend
  surface, or adds automation/hooks/routines → reject or reclassify.

GATE RESULT: pass | blocked (reason)

## 2. Candidate classification

| # | candidate (my words) | bucket | one-line rationale | safety/scope: pass/flag/block |
|---|---|---|---|---|
|   |   |   |   |   |

Buckets: 1 reject · 2 keep as lesson · 3 AIOS workflow improvement ·
4 EntryLens product/spec/test candidate (HUMAN-REVIEW ONLY) · 5 content/GTM ·
6 ADR candidate · 7 claim-audit/source-quality follow-up · 8 unsafe/blocked.

## 3. One best approved move

- chosen candidate: <#>   bucket: <n>
- selection rule applied: safety/scope → critical path → smallest/reversible →
  clear proof → not sprawl
- rejected alternatives (why not now): <list>
- if no candidate qualifies as a safe build, the move is a non-build action.

## 4. Handoff packet

Emit the one type that matches the chosen bucket:

- build move: complete plan-first Claude Code prompt
- human-review-only bucket 4: SPEC-INPUT NOTE, not code
- ADR bucket 6: ADR stub
- content/GTM bucket 5: content brief
- follow-up bucket 7: claim/source audit request
- reject/lesson-only/blocked buckets 1/2/8: disposition only

## 5. Proof + logging instructions

- proof downstream execution must produce:
- indexes to update:
- proof-log destination:
- linkage: brief_id ↔ source_lesson ↔ resulting_adr ↔ resulting_proof_log

## 6. Audit

- self-check: "no trading advice; no EntryLens runtime code in AIOS; no
  predicate/replay promoted (flag only); no automation/hooks/routines added;
  approved_by unset until a human signs."
