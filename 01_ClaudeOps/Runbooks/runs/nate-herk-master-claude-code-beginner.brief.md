---
brief_id: LDB-20260621-01
date: 2026-06-21
reviewer: Alexander Minnick
source_lesson: user-supplied Nate Herk source — uploaded CLAUDE.md ("WAT framework: Workflows/Agents/Tools") + PDF "Master 95% of Claude Code as a beginner"; no committed lesson packet exists in 06_YouTube-Lesson-Library/
source_rights_status: amber
status: draft
approved_by:
approved_date:
resulting_adr:
resulting_proof_log:
---

# Lesson Decision Brief — Nate Herk, "Master 95% of Claude Code as a beginner" (WAT framework)

> Filled by hand using `01_ClaudeOps/Runbooks/lesson-decision-brief.md`.
> Acceptance run for the runbook. Source grounded **only** in the readable
> uploaded `CLAUDE.md` (the WAT framework); the accompanying PDF was not
> machine-readable in this environment (see candidate #5). Raw uploads were not
> committed; all content below is paraphrased.

## 0. Input integrity (gate — fail closed)

- lesson packet present: **yes** — user supplied the Nate Herk source directly
  (readable `CLAUDE.md` / WAT framework + an image-based PDF), assessed inline
  for this run. No pre-existing committed packet in `06_YouTube-Lesson-Library/`
  (noted; out of scope to create one here).
- required front-matter complete: **yes (for integrity)** — source identity
  (title; creator Nate Herk; platform YouTube; type: video + example CLAUDE.md),
  `rights_status`, claim-audit status, and a source-quality verdict are all
  established below. Non-blocking metadata gaps: `published_date` unknown; no
  formal 0–20 rubric score (no packet) → routed to bucket (7).
- claim-audit status: **pending** — the readable CLAUDE.md is instructional (an
  agent-architecture SOP), not a set of external factual claims; the PDF's
  specific claims could not be read here, so a full claim-audit is deferred
  (bucket 7).
- source-quality score: **qualitative: Accept with restrictions (paraphrase
  only)** — known creator, instructional Claude-Code content; numeric rubric not
  run (no packet).
- rights_status: **amber** — creator's copyrighted material; "free to watch" is
  not a reuse license. Paraphrase-only private notes permitted; no verbatim
  reproduction, no raw upload committed.

RULE applied: `rights_status` is **amber** (not red) and the required integrity
fields are populated → Gate 0 **PASSES**. (Buckets are not restricted to
(7)/(8).)

## 1. Safety + scope gate

- trading material in lesson? **no** — this is a beginner Claude Code tutorial
  plus a general AI-agent architecture (WAT = Workflows / Agents / Tools). No
  market, instrument, predicate, signal, or trade content. EntryLens "Green" is
  not referenced.
- product-scope: no candidate adds EntryLens runtime to AIOS, makes EntryLens a
  scanner/signal/dashboard/broker/assistant, adds a frontend, or adds
  automation/hooks/routines. The one EntryLens-adjacent idea (WAT's
  deterministic-execution layer) is treated as conceptual validation only — no
  code, no predicate, no promotion (see §3 rejected alternatives).

GATE RESULT: **pass** (non-trading source; no scope drift).

## 2. Candidate classification

| # | candidate (my words) | bucket | one-line rationale | safety/scope: pass/flag/block |
|---|---|---|---|---|
| 1 | WAT principle: let probabilistic AI handle reasoning/coordination, let deterministic code handle execution | 2 keep as lesson | External validation of AIOS's own line — EntryLens "Green" is deterministic-engine output, never AI-computed; this is confirmation, not a new predicate | pass |
| 2 | "Check for an existing tool before building a new one" | 3 AIOS workflow improvement | Matches `repo-hygiene` reuse-before-build; a candidate for a light doc reinforcement | pass |
| 3 | Self-improvement loop: identify what broke → fix → verify → update the SOP → move on | 2 keep as lesson | Already this repo's proof-log / compounding-loop discipline; nothing new to build | pass |
| 4 | Secrets only in `.env` (never elsewhere), credentials gitignored; don't overwrite SOPs without asking | 2 keep as lesson | Validates existing `repo-hygiene` (no secrets in repo) and plan-first / human-in-the-loop rules | pass |
| 5 | The accompanying PDF ("Master 95% of Claude Code as a beginner") is image-based and was not machine-readable in this environment (no OCR/PDF tooling), so its specific content and any factual/performance claims are unaudited | 7 claim-audit / source-quality follow-up | Cannot audit what cannot be read; needs a text/transcript rendition before any video-specific claim is trusted or promoted | pass |

Buckets used: **2, 3, 7** (5 candidates, 3 buckets).

## 3. One best approved move

- chosen candidate: **#5**   bucket: **7** (claim-audit / source-quality follow-up)
- this is a **non-build action** — no safe executable build is warranted; the
  WAT material validates existing doctrine rather than revealing a gap.
- selection rule applied: **safety/scope** (all candidates pass) → **critical
  path** (none directly unblock Plan Declaration → predicate → engine → meter;
  the follow-up best serves the AIOS compounding loop by closing a source gap) →
  **smallest/reversible** (asking for a readable source is the smallest, fully
  reversible step) → **clear proof** (proof = a readable rendition exists, or a
  confirmation it adds nothing beyond the CLAUDE.md) → **not sprawl** (adds no
  new surface).
- rejected alternatives (why not now):
  - **#2 (bucket 3 doc cross-reference)** — `repo-hygiene` already encodes
    reuse-before-build; a new note would be sprawl with no clear proof gain.
  - **#1 / #3 / #4 (bucket 2)** — lesson-only; they confirm existing doctrine,
    so no action beyond recording the lesson.
  - **An EntryLens (bucket 4) flag** — considered for WAT's deterministic layer,
    but rejected: it is conceptual validation, not a deterministic predicate or
    replay fixture, so there is nothing to flag; and promotion is human-only via
    the locked gate regardless.
- "Never manufacture a build" honored: the move is a follow-up, not a build.

## 4. Handoff packet

Type: **follow-up (bucket 7) — claim/source audit request.**

- Supply a **text/transcript rendition** of the PDF "Master 95% of Claude Code
  as a beginner" (paraphrased notes or a transcript), since it could not be read
  here (image-based; no PDF/OCR tooling in this environment).
- Then run `source-quality` and `claim-audit` on any video-specific factual or
  performance claims before treating them as usable.
- Only if a genuinely new, in-scope candidate emerges should this runbook be
  re-run on the fuller source. No build, no EntryLens promotion, and no
  automation is requested.
- Bucket-2 items (#1, #3, #4): disposition = **lesson-only**, no action.

## 5. Proof + logging instructions

- proof downstream execution must produce: a readable rendition (or paraphrased
  notes) of the PDF, plus a claim-audit verdict on any video-specific claims;
  if any future candidate is ever promoted, the proof that the change works.
- indexes to update: none required for this non-build run beyond the Runbooks
  index already recording it. If the follow-up yields a proof log, add a row to
  `00_System/Proof-Index.md`; if anything later rises to a durable decision, add
  a row to `00_System/Decision-Index.md` (ADR-NNNN).
- proof-log destination: `09_Proof-Logs/` (written with `Templates/proof-log.md`).
- linkage: `brief_id LDB-20260621-01` ↔ `source_lesson` (Nate Herk WAT CLAUDE.md
  + PDF) ↔ `resulting_adr` (none) ↔ `resulting_proof_log` (none yet).

## 6. Audit

- self-check: "no trading advice; no EntryLens runtime code in AIOS; no
  predicate/replay promoted (flag only); no automation/hooks/routines added;
  approved_by unset until a human signs."
