---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §10
---

# Permissions Index

## What belongs here

The rationale behind every deny/ask/allow rule enforced in
`.claude/settings.json` — this file explains *why*; that file is what
actually runs.

## What does not belong here

The enforcement rules themselves. `.claude/settings.json` is the single
source of truth for what's actually denied/asked/allowed; if this file and
that file disagree, the settings file is what's really enforced and this
file is stale.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §10 (Settings / security baseline) and
`.claude/settings.json` (the live enforcement).

---

## Philosophy

Enforcement, not preference. Default mode is `plan`. Permissions exist so
that safety doesn't depend on Claude remembering a sentence in a prompt —
see `Master-Blueprint-V1.md` §13, "false safety from prompts."

## Why each category is ruled the way it is

| Category | Rule | Why |
|---|---|---|
| `.env`, secrets, credentials, tokens | Deny read | Never let model context include live secrets |
| broker, accounts, pnl, live-account | Deny read | Trading-safety boundary — see `00_System/Safety-Policy.md` |
| raw transcripts, raw artifacts | Deny read/write | Raw material is never the brain — see `Promotion-Rules.md` |
| `git push --force`, `reset --hard`, `clean -fd`, `rm -rf` | Deny | Irreversible destructive operations |
| piping curl/irm/iwr into a shell | Deny | Arbitrary remote code execution |
| broker/order/buy/sell-named commands | Deny | Trading-safety boundary, belt-and-suspenders with doctrine |
| commits, pushes, installs, Docker, `gh`, browser/Playwright/Chrome | Ask | Visible-to-others or environment-changing actions need a human in the loop |
| `git status/diff/log`, test/lint/typecheck runs, scoped scan/test scripts | Allow | Read-only or already-sandboxed |

## Adding a new rule

Propose it here first with rationale, then mirror it into
`.claude/settings.json`. Any new `allow` rule for a Bash pattern should be
as narrow as the table above, not a broad wildcard.
