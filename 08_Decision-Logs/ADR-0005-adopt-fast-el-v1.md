# ADR-0005 — Adopt FAST_EL_V1 — Lane B default, Router guards meaning

```yaml
id: ADR-0005
date: 2026-06-24
owner: bigmannot23
status: accepted        # proposed | accepted | superseded
proof_required: no
```

**Status:** Accepted (2026-06-24). Status is recorded in prose because
`Templates/decision-log.md` and `00_System/Decision-Index.md` carry no
`status` field — see `.claude/rules/decision-ledger-discipline.md` (known
gap).

**Decision:**

Adopt **FAST_EL_V1** as Router behavior for EntryLens execution.

- **Default to Lane B** (single-pass build) when meaning is pinned by repo
  truth or written human approval. Never pinned from memory alone.
- **Lanes:**
  - **A = meaning** — human-approved, read-only / scoped.
  - **B = build** — one bounded run: code + tests + fixtures + replay +
    repairs + proof.
  - **C = mechanical** — may skip Router; micro proof.
  - **D = spike** — branch only, no production semantics.
- **STOP and state-check are interrupts, not standing lanes.** Quick
  state-check only when target/blocker is uncertain or moving A→B; full
  reconciliation only on conflict, semantic change, or post-merge update.
- **Prompt compression via macro tags;** full macro definitions live in
  EntryLens `CLAUDE.md`.
- **Three proof receipts:** micro / standard / high-risk.
- **Anti-process gate:** no AIOS work unless it clears a current blocker,
  removes a repeated step, or records accepted proof.

**Context:**

EntryLens execution was moving too slow. The cause was **process drag** —
doctrine re-pasted every prompt, state re-checked every session, one slice
split across many prompts, AIOS work between PCs — **not the safety rules**.
Execution was over-restricted; meaning was not.

**Unchanged (hard carve-outs):**

EntryLens stays deterministic plan-alignment, **not** signals /
recommendations / a scanner. **Green = every locked declared condition
deterministically true now; Green is not buy.** Status semantics, N-of-M,
fail-closed, replay lock, no-signal language, and doctrine / ADR / state
edits all remain human-pinned and strict. Missing / stale / ambiguous data
fails closed.

**Enforcement:**

Macros and `CLAUDE.md` are **context, not blocks**. Hard prevention of
protected-file edits and banned language is done by **PreToolUse hooks**
(added next).

**Guardrails:**

1. "Lane B default" requires pinned meaning in repo or written approval —
   **never memory**.
2. Loosening the human gate is only safe once the **hook gate exists**.
3. Adopt as **Router behavior**, not a new infrastructure project.

**Alternatives rejected:**

- *Keep the over-restricted process* (re-paste doctrine every prompt,
  re-check state every session, split one slice across many prompts) —
  rejected: that process was the source of the drag, and the safety rules
  were never the bottleneck.
- *Loosen the meaning / safety rules to gain speed* — rejected: the
  carve-outs above stay strict; speed comes from compressing process, not
  meaning.
- *Loosen the human gate now, before hooks exist* — rejected per Guardrail
  2; the gate only relaxes once PreToolUse hooks enforce protected files and
  banned language.
- *Stand up a new infrastructure project* — rejected per Guardrail 3; this
  is adopted as Router behavior.

**Rationale:**

The bottleneck was process, not safety, so the fix compresses process while
leaving meaning pinned and the hard carve-outs untouched. Lane B collapses a
bounded build into a single pass; state-checks become interrupts instead of
standing overhead; macro tags shrink prompt cost without changing what the
words mean. Loosening the human gate is deferred until the hook gate makes it
safe.

**Consequence / metric:**

Track **prompts per merged PC** (goal: down) and **semantic escapes** (goal:
zero). Router instructions and EntryLens `CLAUDE.md` updated accordingly.

**Proof required** (link to `09_Proof-Logs/_index.md` entry once available):

No. This is a Router-behavior / doctrine adoption, not a behavioral code
change that needs a replay. (The micro / standard / high-risk proof receipts
are a mechanism FAST_EL_V1 introduces for Lane B builds — separate from
whether this ledger entry needs a proof-log link.)
