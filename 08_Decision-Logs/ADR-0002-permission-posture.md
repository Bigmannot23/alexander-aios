# ADR-0002 — Permission Posture

```yaml
id: ADR-0002
date: 2026-06-21
owner: bigmannot23
status: accepted        # proposed | accepted | superseded
proof_required: no
```

**Status:** Accepted (2026-06-21). Status is recorded here in prose because
`Templates/decision-log.md` and `00_System/Decision-Index.md` do not yet carry
a `status` field — see `.claude/rules/decision-ledger-discipline.md` (known
gap).

**Context:**

The first real YouTube-ingest run digested Nick Saraev's "Claude Code Full
Course" (2026-02-12). See the sanitized notes — by reference only, no raw
transcript:

- `06_YouTube-Lesson-Library/packets/2026-02-12-saraev-claude-code-full-course.md`
- `06_YouTube-Lesson-Library/lessons/2026-02-12-saraev-claude-code-full-course.md`

That course uses Claude Code's `dangerously-skip-permissions` bypass mode as a
*default* working mode, and recounts a firsthand incident in which a
destructive terminal command issued under broad permissions bricked a drive.
Both the packet and the lesson flagged the repo's response as a decision-log
candidate: record an explicit stance on `dangerously-skip-permissions`,
proposed as "keep manual permission gating; do not adopt bypass as default."

Until now that posture has been only implicit. `.claude/rules/claude-surface-routing.md`,
`.claude/rules/repo-hygiene.md`, and the `.claude/settings.json` permission
model all assume manual gating, but no decision log names the permission *mode*
by name. This ADR fixes the stance as a durable, defensible record. It is
doctrine capture, not a behavioral or runtime change.

**Decision:**

Keep **manual permission gating** as the default working mode for all work in
this repo. Do **not** adopt `dangerously-skip-permissions` (or any equivalent
permission-bypass mode) as a default. Status: **accepted.**

A permission-bypass mode may be used only when *all* of the following hold:

1. it is a deliberate, explicitly human-authorized action for a single,
   narrowly-scoped task;
2. the task is **non-trading and non-account** — it touches no broker, account,
   order, P&L, payment, banking, or live-market surface, and no
   trading/market/account work whatsoever; and
3. the blast radius is understood and bounded — no broad credential grants, no
   auto-run of destructive commands, nothing that could exfiltrate or destroy
   data outside the intended target.

Bypass mode is **never** permitted for trading, market, or account work — no
exception, and no accumulation of clean runs ever "earns" it. This restates, at
the permission-mode level, the unconditional bans already in
`.claude/rules/entrylens-trading-safety.md` and the automation ceiling in
`.claude/rules/alexander-aios-boundary.md` (nothing trading/market/account-related
is ever automated or bypass-run, regardless of how many clean runs it
accumulates).

**Alternatives rejected:**

- *Adopt `dangerously-skip-permissions` as a default* (the source's practice) —
  rejected. The destructive blast radius is unbounded and conflicts with the
  repo's safety posture; the source's own bricked-drive story is the cautionary
  evidence.
- *Leave the posture implicit* — rejected. An unwritten default is not a
  decision; `.claude/rules/decision-ledger-discipline.md` requires durable
  boundary decisions to be logged, not left in chat or inferred from settings.

**Rationale:**

Manual permission gating is the repo's primary blast-radius control, and the
maximal-safety default that still lets work proceed: every high-impact action
gets a human checkpoint, while routine read-only and clearly-bounded edits stay
fast. The convenience of skipping prompts does not justify the downside — a
single mistaken destructive command, run without a confirmation gate, is exactly
the failure the source's anecdote demonstrates. Manual gating also composes with
the repo's other hard lines: the secrets/raw-data ban in `repo-hygiene.md` and
the no-broker/account rule in `entrylens-trading-safety.md` both depend on no
high-impact action running unattended. It matches the automation ladder (manual
prompt → skill → proven script → hook) and the no-autonomy posture of ADR-0001 —
the brain enables and informs; it does not act unattended. Naming the stance now
prevents quiet drift toward bypass-as-default as the repo accumulates skills and
scripts.

**Consequences / tradeoffs:**

- More permission prompts and a slower flow than bypass-by-default.
- In exchange: bounded blast radius, no unattended destructive actions, and an
  auditable, defensible record of why.
- Any future proposal to relax this (e.g. a scoped bypass for one specific
  proven workflow) must come through a new decision log, never a silent
  settings change.

**Relationship to ADR-0001:**

Independent of, and consistent with, ADR-0001 (trader-intelligence company).
This ADR does not amend or supersede it; it records a separate boundary (the
permission mode) that reinforces the same no-autonomy, safety-first posture. No
supersession link is created.

**Supersession policy:**

If this stance changes later, do not silently rewrite this entry. Write a new
ADR that supersedes ADR-0002, link both directions (this entry → the new one,
and the new one → this entry), and mark this entry's status `superseded` while
leaving its text intact as history, per
`.claude/rules/decision-ledger-discipline.md`.

**Proof required** (link to `09_Proof-Logs/_index.md` entry once available):
No. This is a doctrine record, not a behavioral change that needs evidence of
working.
