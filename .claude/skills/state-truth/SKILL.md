---
name: state-truth
description: Read-only git state derivation run at the START of every Claude Code session in this repo. Derives real repo truth from git (branch, working tree, HEAD vs origin/main drift, recent commits, unmerged branches) and the actual ADR/PC directory (highest existing identifier), then emits a fixed-format STATE REPORT. Replaces hand-written REPO STATE headers that go stale. Trigger at session start, or any time you must confirm true repo state before acting. NEVER writes, commits, or modifies anything.
---

# state-truth

Run this FIRST, before trusting any handoff note, REPO STATE header, or chat narrative.
Chat is not truth; git is. This skill derives truth from git and the filesystem, read-only,
and prints a STATE REPORT you can paste into the Command Router or hand to a session in the
OTHER repo (that is how cross-repo state gets shared without reading the other repo).

## Hard rule
READ-ONLY. Run only commands that read. Never write, stage, commit, push, amend, checkout,
reset, or merge. If a step would require a write, STOP and report — do not proceed.

## Steps (run in order, report each)

1. git fetch --all --prune
2. git status -sb
   (branch, ahead/behind origin, clean vs dirty, untracked files)
3. git rev-parse --short HEAD ; git rev-parse --short origin/main ;
   git rev-list --left-right --count origin/main...HEAD
   (HEAD hash, origin/main hash, drift each direction)
4. git log --oneline -10
5. git branch --no-merged main ; git branch -r --no-merged main
   (branches not yet merged to main; "none" if all merged)
6. ADR / decision dir truth (derive identifiers, never assume):
   for d in docs/adr docs/adrs adr 08_Decision-Logs decisions; do [ -d "$d" ] && { echo "== $d =="; ls "$d"; }; done
   (report which dir exists and the HIGHEST ADR/EL-ADR id actually present)
7. PC register, if present:
   [ -f 04_AlphaLab/Predicate-Candidates/_index.md ] && grep -oE 'PC-[0-9]{4}' 04_AlphaLab/Predicate-Candidates/_index.md | sort -u | tail -1
   (highest PC-NNNN, or "no PC register in this repo")
8. Known indices, if present:
   for f in 00_System/Decision-Index.md 08_Decision-Logs/_index.md docs/audits/V1-ENGINE-INTAKE-AUDIT.md; do [ -f "$f" ] && echo "present: $f"; done

## Emit this STATE REPORT (fixed format, fill from above)

=== STATE-TRUTH REPORT ===
repo:            <from `git remote get-url origin`>
branch:          <branch>  (<clean|dirty>, ahead <n> / behind <n>)
HEAD:            <short>   origin/main: <short>   drift: <main +n / HEAD +n>
recent:          <top 3 of git log --oneline>
unmerged:        <branches not merged to main, or "none">
adr dir:         <which dir exists>  highest ADR: <id or none>
pc register:     <highest PC-NNNN or "none in this repo">
indices present: <list>
notes:           <anything surprising vs the handoff you were given>
=== END REPORT ===

## When state contradicts a handoff
Trust this report. Flag the contradiction in notes:. Do not reconcile from narrative.
