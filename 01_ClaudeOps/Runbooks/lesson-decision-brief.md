---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §1.1, §1.4, §6, §7, §12
stage: runbook / manual-proof
---

# Runbook — Lesson Decision Brief

A manual SOP for turning **one completed lesson packet** into **one safe
decision/handoff**:

`lesson packet → lesson-decision-brief → one best move (or no-build disposition) → proof/logging instructions`

Run this by hand. It is **not** a skill, **not** a script, **not** a hook or
routine, and it produces **no runtime code**. Fill one copy of
`Templates/lesson-decision-brief.output.md` per run and save it to
`01_ClaudeOps/Runbooks/runs/<source-slug>.brief.md`.

## Source of truth — read first

- `00_System/Safety-Policy.md`
- `OPERATING_DOCTRINE.md`
- `00_System/Master-Blueprint-V1.md` (§1.1 Green, §1.4 locks, §6 safety, §7 learning refinery, §12 automation ladder)
- `.claude/rules/entrylens-trading-safety.md`, `.claude/rules/alexander-aios-boundary.md`
- The lesson packet being processed, plus `00_System/Decision-Index.md` / `00_System/Proof-Index.md` context if present

The blueprint wins on every conflict.

---

## 1. Purpose

Turn one completed YouTube/research **lesson packet** into one safe decision or
handoff. The output is a single best move — or, if no safe build qualifies, an
explicit non-build disposition — plus instructions for proving and logging
whatever happens next.

- **Scope is lesson packets only.** This runbook consumes the output of an
  ingest workflow (e.g. a `06_YouTube-Lesson-Library/` packet, or a
  research-ingest note). It does not ingest raw sources, score source quality,
  or audit claims — those are upstream skills.
- **"Promotion review" here means internal triage, not the locked EntryLens
  Promotion gate.** When this runbook sorts a candidate into the EntryLens
  bucket, it is doing internal triage to decide *whether a human should look at
  it later*. That is a different thing from the locked EntryLens Promotion
  gate, which is the only path by which a candidate can actually become
  EntryLens truth.
- **This runbook can only flag EntryLens candidates; it cannot promote them.**
  The locked Promotion gate (Master-Blueprint §1.4, §4, §6.4) requires a
  manually approved promotion packet + spec delta + tests, decided by a human.
  This runbook never satisfies, shortcuts, or stands in for that gate. Its
  strongest possible action on an EntryLens candidate is a HUMAN-REVIEW-ONLY
  flag.

## 2. Inputs

- **Lesson packet** — the completed, sanitized lesson/packet file.
- **Proof logs** — any existing `09_Proof-Logs/` evidence relevant to the packet.
- **Claim / source-quality status** — the packet's claim-audit result and
  source-quality verdict (and `rights_status`).
- **Product-scope / trading-safety doctrine** — the `.claude/rules/` files,
  `00_System/Safety-Policy.md`, and blueprint §1 and §6.
- **Decision-Index / Proof-Index** — `00_System/Decision-Index.md` and
  `00_System/Proof-Index.md` if present, for linkage and to avoid
  re-litigating settled decisions.

## 3. Gate 0 — Input integrity (fail closed)

Check, before anything else:

- **Packet present?**
- **Required front matter complete?** (source identity, dates, `rights_status`)
- **Claim-audit status** — done | pending | n/a.
- **Source-quality score / verdict** — value present, or missing.
- **`rights_status`** — green | amber | red.

**Fail closed.** If `rights_status` is **red** OR any required field is
**missing**, the only buckets you may use are:

- **(7) claim-audit / source-quality follow-up**, and
- **(8) unsafe / blocked**.

Classify into those, write the disposition, and stop. Do not proceed to
candidate selection on untrustworthy or incomplete input. (This mirrors the
blueprint's Gray = fail-closed rule, §1.3 / §7.2.)

## 4. Gate 1 — Safety + scope

If the lesson contains **trading material**:

- Any candidate **predicate / setup / no-trade / replay** item is
  **HUMAN-REVIEW ONLY**. It **cannot** be the approved build move. It routes to
  a **promotion-candidate flag** only — never an implementation packet, never
  code, never a promotion.
- Any **advice / signal / probability / win-rate / edge / sizing / contract /
  entry / exit / broker / account / P&L** content → **bucket (8) unsafe /
  blocked**. Do not soften it into "educational" framing.

**Product-scope.** Reject or reclassify any candidate that would:

- add EntryLens **runtime code inside Alexander-AIOS** (engine code lives in the
  separate `entrylens/` repo, never here),
- make EntryLens a **scanner / signal generator / dashboard / broker / trading
  assistant**,
- add a **frontend surface**, or
- add **hooks / routines / automation** (the automation ceiling is manual
  prompts and skills only).

Record the gate result: **pass** or **blocked (reason)**.

## 5. Eight classification buckets

Sort every candidate you extract from the packet into exactly one bucket:

1. **reject** — not useful; drop it (record why).
2. **keep as lesson only** — useful understanding, no action beyond the lesson.
3. **AIOS workflow improvement** — a safe, in-scope improvement to this brain
   (docs, skills, indexes, templates, runbooks).
4. **EntryLens product / spec / test candidate** — **HUMAN-REVIEW ONLY, no
   code, no auto-promotion.** Flag only; routes toward the locked Promotion
   gate for a human to decide.
5. **content / GTM candidate** — a content, teaching, or go-to-market idea.
6. **ADR candidate** — a durable decision worth an Architecture Decision Record
   (`08_Decision-Logs/`).
7. **claim-audit / source-quality follow-up** — needs sourcing, auditing, or a
   cleaner source before it can be trusted or promoted.
8. **unsafe / blocked** — crosses a safety or scope line; stop.

## 6. Single best move

Choose exactly one move, in this priority order:

1. **safety / scope passes** (never choose a flagged or blocked candidate),
2. **unblocks the critical path** — Plan Declaration → predicate → engine →
   meter (the EntryLens build path; aligns to the §1.2 nine-condition
   plan-completeness gate), or the AIOS compounding loop (decisions logged,
   proof captured, repeated prompts → skills),
3. **smallest / reversible**,
4. **has clear proof**,
5. **avoids feature sprawl**.

**Never manufacture a build. If no candidate qualifies as a safe executable
move, the chosen move is a non-build action: lesson-only, ADR, follow-up, or
blocked.**

## 7. Output template usage

Fill one copy of `Templates/lesson-decision-brief.output.md` per run:

- **Front matter** — assign `brief_id: LDB-<YYYYMMDD>-<nn>`, the date, the
  reviewer, `source_lesson` (path/source_id of the packet), and
  `source_rights_status`. Leave `status: draft`, and leave `approved_by`,
  `approved_date`, `resulting_adr`, `resulting_proof_log` **blank** until a
  human signs and downstream work happens.
- **§0 Input integrity** — record Gate 0 results; apply the fail-closed rule.
- **§1 Safety + scope** — record the Gate 1 result.
- **§2 Candidate classification** — one row per candidate (your paraphrase,
  bucket, one-line rationale, safety/scope: pass/flag/block).
- **§3 One best approved move** — the chosen candidate + bucket, the selection
  rule applied, rejected alternatives, or the non-build disposition.
- **§4 Handoff packet** — emit the one handoff type that matches the chosen
  bucket (see the template).
- **§5 Proof + logging** — what downstream execution must prove, which indexes
  to update, the proof-log destination, and the linkage chain.
- **§6 Audit** — keep the self-check line.

Save to `01_ClaudeOps/Runbooks/runs/<source-slug>.brief.md`.

## 8. Graduation criterion

Promote this runbook to a future generic
`.claude/skills/decision-brief/SKILL.md` only after **3–5 clean manual runs
with zero safety/scope misfires**. Until then it stays a manual runbook. Do not
build a skill, script, hook, or routine for it before that bar is met.
