---
name: state-truth
description: Read-only git state derivation run at the START of every Claude Code session in this repo. Derives real repo truth from git (branch, working tree, HEAD vs origin/main drift, recent commits, unmerged branches) and the actual ADR/PC directory (highest existing identifier), then emits a fixed-format STATE REPORT. Replaces hand-written REPO STATE headers that go stale. Also offers an additive header mode that emits ONE paste-ready, fenced REPO STATE block (repo, branch, working tree, last 5 commits, ADR filenames, the live predicate register, and the active blocker) for dropping into future prompts. Trigger at session start, or any time you must confirm true repo state before acting. NEVER writes, commits, or modifies anything.
---

# state-truth

Run this FIRST, before trusting any handoff note, REPO STATE header, or chat narrative.
Chat is not truth; git is. This skill derives truth from git and the filesystem, read-only,
and prints a STATE REPORT you can paste into the Command Router or hand to a session in the
OTHER repo (that is how cross-repo state gets shared without reading the other repo).

## Modes

- **default (full report)** — run the numbered Steps below and emit the
  `=== STATE-TRUTH REPORT ===` block. This is the session-start mode; nothing
  about it changes.
- **header** — emit ONE paste-ready, fenced REPO STATE block (repo, branch,
  working tree, last 5 commits, ADR filenames, the live predicate register,
  and the active blocker) for dropping into a future prompt, the Command
  Router, or a handoff. See **## Header mode** at the bottom. Additive: it
  reuses the same read-only sources and adds no new derivation to the default
  report.

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

## Header mode (additive — paste-ready REPO STATE block)

Use when you want a compact block to paste into a future prompt, the Command
Router, or a handoff — not the full session-start report. Same hard rule:
READ-ONLY. Every field is derived live from git / the filesystem at run time;
nothing is hand-typed. If a source path is missing or a field can't be derived,
print `UNRESOLVED` — never fabricate a value.

This is the AIOS counterpart to the EntryLens header mode. The difference: in
AIOS the **predicate register** and the **active blocker** actually live in this
repo, so they are real derived fields here, not pointer lines.

Field → source (all live, this repo, read-only):

| Field | Source |
|---|---|
| repo + branch | `git remote get-url origin` ; `git rev-parse --abbrev-ref HEAD` |
| working tree | `git status --short` (or `clean`) |
| last 5 commits | `git log -5 --pretty='%h %s'` |
| ADR filenames | ADR dir if one exists here (same dir candidates as Step 6); else the literal line `ADRs: see entrylens-platform` |
| predicate register | `04_AlphaLab/Predicate-Candidates/_index.md` — the PC-NNNN register lives HERE, so this is real content, not a pointer line |
| active blocker | `00_System/Blockers-Debt-Ledger.md`, filtered to `BLOCKS-BUILD` items not marked `RESOLVED` |

Run this read-only snippet from the repo root; its stdout IS the block to paste:

````bash
{
  repo=$(basename -s .git "$(git remote get-url origin)" 2>/dev/null || echo UNRESOLVED)
  branch=$(git rev-parse --abbrev-ref HEAD 2>/dev/null || echo UNRESOLVED)
  echo '```'
  echo "REPO STATE — ${repo} @ ${branch}"
  echo "source: state-truth header mode — live from git/fs, read-only"
  echo
  echo "branch:  ${branch}"
  st=$(git status --short)
  if [ -z "$st" ]; then echo "status:  clean"; else echo "status:"; echo "$st" | sed 's/^/  /'; fi
  echo
  echo "recent (last 5):"
  git log -5 --pretty='  %h %s' 2>/dev/null || echo "  UNRESOLVED"
  echo
  adrdir=""
  for d in docs/adr docs/adrs adr 08_Decision-Logs decisions; do [ -d "$d" ] && { adrdir="$d"; break; }; done
  if [ -n "$adrdir" ]; then
    echo "ADRs (${adrdir}):"
    ls "$adrdir"/ADR-*.md 2>/dev/null | xargs -n1 basename 2>/dev/null | sed 's/^/  /' || echo "  UNRESOLVED"
  else
    echo "ADRs: see entrylens-platform"
  fi
  echo
  reg=04_AlphaLab/Predicate-Candidates/_index.md
  if [ -f "$reg" ]; then
    ids=$(grep -oE 'PC-[0-9]{4}' "$reg" | sort -u)
    if [ -n "$ids" ]; then
      echo "predicate register (${reg}):"
      echo "  $(echo "$ids" | paste -sd, -) — $(echo "$ids" | grep -c .) candidates, highest $(echo "$ids" | tail -1)"
    else
      echo "predicate register (${reg}): UNRESOLVED (no PC-NNNN rows found)"
    fi
  else
    echo "predicate register: UNRESOLVED (${reg} not found)"
  fi
  echo
  ledger=00_System/Blockers-Debt-Ledger.md
  if [ -f "$ledger" ]; then
    echo "active blocker(s) — BLOCKS-BUILD, unresolved (${ledger}):"
    out=$(awk '
      /^## BD-/ { hdr=substr($0,4); resolved=($0 ~ /RESOLVED/) }
      /^- \*\*Tag:\*\*/ { if (hdr!="" && $0 ~ /BLOCKS-BUILD/ && !resolved) { print "  " hdr; hdr="" } }
    ' "$ledger")
    [ -n "$out" ] && echo "$out" || echo "  none active"
  else
    echo "active blocker: UNRESOLVED (${ledger} not found)"
  fi
  echo '```'
}
````

The emitted block is self-contained: paste it verbatim into the next prompt.
Re-run any time the tree changes — it never goes stale because it holds no
hand-typed state. The active-blocker filter treats a heading-level `RESOLVED`
marker as cleared (the ledger's own convention); a future blocker marked
resolved only in prose would still show until its heading is tagged.
