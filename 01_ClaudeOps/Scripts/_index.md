---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §9, §12
---

# Scripts Index

## What belongs here

Every manual scan/test script for this repo, one row each, with a clean-run
counter. Scripts are the step *before* a hook — they must be run manually
5–10 times cleanly before they're eligible to become a hook.

## What does not belong here

Hooks (see `01_ClaudeOps/Hooks/README.md` — none exist yet) or routines
(see `01_ClaudeOps/Routines/README.md` — none exist yet). Product runtime
code of any kind.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §9.1 work block 4 ("settings.json + scan stubs")
and §12 (automation ladder: "no hook before 5–10 clean manual runs").

---

## Ledger

No scripts exist yet. Add a row per script using this format:

| Script | Purpose | Clean manual runs | Hook-eligible? | Link |
|---|---|---|---|---|
| _none yet_ | | 0 / 10 | No | |

Allowed script locations once they exist: `scripts/scan/*.ps1` and
`scripts/test/*.ps1`, matching the `.claude/settings.json` allow-list.
