---
name: route-task
description: Advisory-only task router for the Alexander-AIOS repo. Trigger on "where does this go," "which repo," "which folder," "which surface," "which model," "route this," "what should I do with this," "is this in the right place," "which phase," or any ambiguous task-placement request. Emits a routing decision only — system, repo, folder, Claude surface, model, allowed/forbidden files, safety class, work class, next prompt. Never performs the routed work, never executes tasks, never edits files outside this skill's own definition.
---

# route-task

`route-task` is a **read-only advisory classifier**. Given any incoming
task, it emits a routing decision — system, repo, folder, Claude surface,
model, allowed files, forbidden files/actions, safety class, work class,
next prompt, defer/stop conditions. It never performs the routed work,
never executes tasks, and never writes outside its own skill files.

## Source of truth — read only

This skill reads, but never edits:

- `00_System/Master-Blueprint-V1.md`
- `00_System/Safety-Policy.md`
- `OPERATING_DOCTRINE.md`
- `CLAUDE.md`
- `CURRENT_STATE.md`
- `.claude/skills/_index.md`
- `01_ClaudeOps/Skills/_index.md`

The blueprint wins on every conflict. (This read-only rule governs the
skill's own routing behavior — it is distinct from the one-time authoring
step that created this file and its index entries.)

## Procedure

Run in order for every incoming task:

1. **Restate** the incoming task in one line.
2. **Identify the SYSTEM:**
   - Alexander-AIOS brain
   - ClaudeOps
   - EntryLens OS
   - AlphaLab OS
   - AlphaLab Live Vision Desk
   - Productization Lab
   - YouTube Lesson Library
   - Course Library
   - Research Library
   - Decision Logs
   - Proof Logs
   - Dashboards
   - Archive
3. **Apply source-of-truth scope:**
   - Brain organization, learning, IP, source quality, surfaces, cadence →
     Master Blueprint / AIOS doctrine
   - EntryLens product and runtime semantics → EntryLens doctrine
   - Live chart, vision, browser, TradingView → Live Vision Desk doctrine
   - Business, GTM, ICP, pricing, acquisition → Productization Lab / GTM
     addendum only
   - If unresolved → route to a decision log instead of deciding in chat
4. **Map SYSTEM to repo and folder:**
   - Brain / doctrine / logs → `alexander-aios`, relevant numbered folder
   - EntryLens pointer/staging → `alexander-aios/03_EntryLens`
   - EntryLens engine/runtime → separate `entrylens` repo later; **DEFER**
     if not created
   - AlphaLab markdown research → `alexander-aios/04_AlphaLab`
   - Live Vision Desk → separate future repo; **DEFER**
   - ClaudeOps machinery → `alexander-aios/01_ClaudeOps`
   - Productization / GTM → `alexander-aios/02_Productization-Lab`
   - YouTube lessons → `alexander-aios/06_YouTube-Lesson-Library`
   - Course material → `alexander-aios/05_Course-Library`
5. **Pick Claude surface:**
   - Chat / Project: doctrine, synthesis, strategy, source classification
   - Claude Code: repo edits, skill files, markdown scaffolds, tests,
     commits
   - Cowork: non-code desk work later
   - Design: UI/visual concepts later
   - Routines: read-only maintenance only, much later
   - Dispatch: remote kickoff only, much later
   - MCP/connectors: later; never broker/account/payment
   - Default: Chat for ambiguous doctrine, Claude Code for approved repo
     edits
6. **Pick model:**
   - Opus: doctrine, architecture, ambiguous safety, major source merge
   - Sonnet: implementation, markdown skill work, routine repo edits
   - Haiku: cheap/simple checks
7. **Run hard-stop checks** (below) before giving any runnable next prompt.

## Hard-stop rules

If any trigger, output STOP or DEFER. Never give a runnable next prompt.

1. Trading advice request (buy/sell, calls/puts, entries/exits, contracts,
   sizing, stops/targets, probability, edge, win rate, "should I take
   this") → **REJECT**, or reroute only to non-advisory AlphaLab framing
2. Anything where a model computes or authorizes EntryLens Green →
   **REJECT**
3. Broker/account/order/P&L/banking/payment/execution surface → **REJECT**
4. Scanner, signal generator, live trade caller, dashboard-as-trading-app →
   **REJECT**
5. EntryLens runtime code requested inside `alexander-aios` → **DEFER** to
   separate future `entrylens` repo
6. Hooks/routines/scheduled/Dispatch before scripts have 5–10 clean manual
   runs → **DEFER**
7. Any trading/market/account routine or Dispatch task → **REJECT**
   permanently
8. Live TradingView/browser/AlphaLab-TV work → **DEFER**
9. Premature Connections/Capabilities/Cadence before Context is stable →
   **DEFER**
10. Business/GTM source trying to override EntryLens doctrine or repo
    structure → **DEFER/RECLASSIFY** to Productization Lab

Blueprint basis: rules 1–4 and 7 → `Master-Blueprint-V1.md` §6 /
`Safety-Policy.md` (universal trading boundary); rule 2 also → §1 (EntryLens
status doctrine); rule 5 → §4 (repo boundary); rule 6 → §12/§15 (automation
ladder); rule 8 → §6.2–6.5 (Live Vision Desk); rule 9 → §16 (build-order
discipline); rule 10 → §3 (source-of-truth hierarchy).

## Output card

Always emit this exact field order:

```
Task:
Classification: System | Work class | Safety class
Target repo:
Target folder/path:
Claude surface + why:
Model + why:
Allowed files:
Forbidden files/actions:
Next prompt OR STOP/DEFER with reason:
Defer/stop conditions:
Source-of-truth reference:
```

**Work class** — use only: Immediate · V1 core · V1 optional · Later ·
Productization candidate · Reject — product drift · Reject — unsafe ·
Defer — blocked by build order.

**Safety class** (trading-adjacent tasks only) — use only: Safe live
cognitive support · Post-session only · Research only · EntryLens promotion
candidate · Requires manual approval · Reject.

## What this skill must never do

- Perform the routed work itself.
- Execute the task it is routing.
- Edit any file other than its own definition during routine invocation.
- Compute or authorize EntryLens Green.
- Produce a runnable next prompt when a hard-stop rule triggers.
