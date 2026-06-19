---
read_first: CLAUDE.md
source_of_truth: OPERATING_DOCTRINE.md §5, §6; Master-Blueprint-V1.md §3, §4, §11, §12
scope: unconditional
---

# Alexander-AIOS Boundary

- This repo is the source of truth. If it matters, it's in a file — not
  just in chat memory.
- Claude Project Chat is for context, synthesis, and review — not where a
  durable decision gets made without also being written down.
- Claude Code is the execution surface for repo edits, skills, scripts, and
  commits.
- `CLAUDE.md` is a short router. It points to doctrine; it never becomes
  the doctrine. Don't paste doctrine content back into it.
- Automation ladder order: skills before scripts, scripts before hooks,
  hooks before routines. Never skip a rung.
- No autonomous merges, no force-push, no bypassing review on durable
  changes.
- EntryLens runtime/engine code belongs in a separate, not-yet-created
  `entrylens/` repo. Never write it here.

Full reasoning: `OPERATING_DOCTRINE.md` §5/§6, `Master-Blueprint-V1.md` §3/§4/§11/§12.
