---
read_first: CLAUDE.md
source_of_truth: OPERATING_DOCTRINE.md §5, §6; Master-Blueprint-V1.md §10, §12, §13
scope: unconditional
---

# Repo Hygiene

- No raw, private, or sensitive files in this repo — secrets, credentials,
  tokens, account/broker/payment data, raw screenshots/transcripts/clips.
  If it's gitignored, it shouldn't be staged either.
- Keep diffs small and reviewable.
- Plan before any multi-file edit — write the plan, get it approved, then
  execute.
- A durable decision (changes doctrine, architecture, or a system boundary)
  becomes a decision log in `08_Decision-Logs/`, not just a chat answer.
- A workflow repeated by hand more than once becomes a skill before it
  becomes a script.
- A deterministic check repeated more than once becomes a script before it
  becomes a hook.
- A script only becomes a hook after 5–10 clean manual runs — never before,
  never for trading/market/account work even then.

Full reasoning: `OPERATING_DOCTRINE.md` §5/§6, `Master-Blueprint-V1.md` §10/§12/§13.
