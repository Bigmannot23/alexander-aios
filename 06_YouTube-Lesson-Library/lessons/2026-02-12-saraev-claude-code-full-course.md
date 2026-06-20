---
source: youtube
video_title: "Claude Code Full Course 4 Hours: Build & Sell (2026)"
video_creator: Nick Saraev
video_url: https://www.youtube.com/watch?v=QoQBzR1NIqI   # reference only; never re-published raw
published_date: 2026-02-12
ingested_date: 2026-06-20
rights_status: amber          # green | amber | red — 00_System/Rights-IP-Gate.md
quality_score: 16             # 0-20 — 00_System/Source-Quality-Rubric.md
---

# Lesson — Claude Code Full Course (Nick Saraev, 2026)

Paired packet: `../packets/2026-02-12-saraev-claude-code-full-course.md`.
Amber rights — paraphrased notes only. Non-trading source.

## Extracted concepts (definitions/mechanics only)
- **`CLAUDE.md` "project brain":** a markdown file injected at the start of every session that steers output; keep it lean (~200–500 lines), guardrails at top, prune regularly, generate a first draft with `/init`.
- **`.claude/rules/`:** modular rule files split out of a monolith `CLAUDE.md` (code style, testing, security, etc.).
- **Config precedence:** global `~/.claude` → project `.claude` → enterprise/managed, merged in order.
- **Permission modes:** ask-before-edits (default), accept-edits, plan mode (read-only), dangerously-skip-permissions (bypass).
- **Plan mode:** read-only research/planning pass before acting.
- **Context tools:** `/context` (token breakdown), `/compact`, `/clear`, auto-compaction; model choice affects cost.
- **Hooks:** scripts auto-fired before/after tool calls (set in settings.json).
- **Skills:** instruction-checklist orchestrators; only front matter loads until invoked (token-efficient).
- **Subagents:** isolated context windows with scoped tools/model (research, reviewer, QA patterns).
- **Agent teams:** a team-lead plus peer instances sharing a task list (high token cost).
- **MCP:** external tool servers ("skills others build for you"); proven flows can be re-implemented as cheaper skills.
- **Chrome DevTools integration / git worktrees / Netlify+Modal deploy:** browser control + screenshots; isolated parallel working dirs; static + backend-endpoint hosting.

## Claims requiring audit (link each to `00_System/Claim-Index.md`)
claim-audit verdict for this source = **Block until sourced**; no claim is
"supported." Logging these to `Claim-Index.md` is a pending **human step**
(not done in this run).
- Income/value claims ($4M/yr, $300k+/mo, $10–15k/mo) — **Unsupported** marketing; the first two are mutually inconsistent.
- Engagement claims (2,000+ students; ~150K YouTube) — **Unsupported**.
- Capability hype ("any site <10 min"; "98–99% skill accuracy") — **Unsupported / unfalsifiable**.
- Pricing/model/version + throughput claims (Pro $17; Opus 4.6; 200k–1M context; "~1,000 emails/min") — **Needs-source / Outdated-risk**, require current docs verification.

## Implementation lessons (what changes in how I operate, not what trade to take)
- Validates this repo's existing design: `CLAUDE.md` as a short router + modular `.claude/rules/` files.
- Reinforces the automation ladder already in doctrine: prompt → skill → (proven) script → hook; do not skip rungs.
- Adopt-worthy disciplines (already compatible): keep the brain lean with guardrails at top; always verify (screenshot/test loop); use plan mode for multi-file work; manage context deliberately and push task-like instructions into on-demand skills.
- **Not** adopted: dangerously-skip-permissions as a default, and broad credential/tool grants to agents — both conflict with this repo's safety posture.

## Productization candidates (the source's ideas — staged only; see `02_Productization-Lab/_index.md`)
Attributed to the creator, **not promoted** by this lesson:
- Lead-generation service (scrape → verify → classify → enrich into a sheet → cold outreach).
- Per-prospect generated mockup sites as B2B lead magnets.
- Knowledge-work automations packaged as skills (email triage, onboarding, deliverables).
- Skills deployed as URL endpoints/webhooks (Modal) for internal/client tooling.
- Explicit constraint from the source itself: keep such apps internal/client-only, not public paid SaaS, without a developer security review.

## Trading/education candidates (deterministic only)
N/A — non-trading source. No setup concepts, no EntryLens-adjacent material.

## Promotion candidates
None. Promotion is a separate human decision via `promotion-rules.md`; nothing
here is copied to `promoted/candidates/`.

## Rejected ideas (and why)
- Income/performance figures as fact → unverified marketing.
- dangerously-skip-permissions as default → repo safety posture (manual permission gating).
- Broad agent credentials / auto-run flows → credential + blast-radius risk.
- Premature hooks/MCP/agent-teams adoption → above the current manual-only ceiling (Defer).

## Proof required (what would need to be true to trust this)
- Technique claims verified against current `code.claude.com/docs` before any are promoted.
- Any automation candidate trialed only after 5–10 clean manual runs are logged (per the ladder).
- A recorded repo decision on permission posture before any bypass-mode experimentation.
