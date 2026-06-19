# Changelog

All notable changes to the Alexander-AIOS brain repo are logged here.
Format: date, change, why. See `08_Decision-Logs/_index.md` for the reasoning
behind any non-obvious change, and `09_Proof-Logs/_index.md` for evidence a
change works.

## 2026-06-18

- Moved `Master-Blueprint-V1.md` from repo root into `00_System/` (content
  unchanged — verified by SHA-256 hash before/after the move) to match the
  canonical path referenced throughout the doctrine.
- Scaffolded the full Alexander-AIOS monorepo structure: root doctrine files,
  `00_System/` constitution, `01_ClaudeOps/` indexes, `02_Productization-Lab`
  pointer, `03_EntryLens/` pointer-only staging, `04_AlphaLab/` markdown-only
  research structure, `05_Course-Library` pointer, `06_YouTube-Lesson-Library/`
  ingest pipeline staging, `07`–`11` indexes, `Templates/`, and `.claude/`
  settings. Documentation, structure, templates, and indexes only — no
  runtime code, no hooks, no routines.

## 2026-06-19

- Authored the first Alexander-AIOS skill, `route-task`
  (`.claude/skills/route-task/SKILL.md`): a read-only advisory classifier
  that routes an incoming task to system, repo, folder, Claude surface, and
  model, with hard-stop rules for trading advice, EntryLens Green, and
  premature automation. It performs no work itself and edits no files
  outside its own definition. Added `.claude/skills/_index.md` to track
  built skills, and marked route-task built in
  `01_ClaudeOps/Skills/_index.md`'s first-skills table.
- Patched the root `.gitignore` (added directly to `main` in commit
  `a960b33`) to close real gaps found on review: missing `tokens/`,
  `artifacts/raw/`, `live-account/`, browser profile/cookie patterns, and
  non-functional raw-data patterns (`transcripts/raw/`, `screenshots/raw/`,
  `uploads/raw/` are root-anchored and reversed-order relative to this
  repo's actual `raw-transcripts/`-style folder convention, so they
  wouldn't have matched real nested folders). Added the doctrine-correct
  `**/raw-transcripts/`, `**/raw-screenshots/`, `**/raw-clips/`,
  `**/raw-uploads/` patterns alongside the existing ones rather than
  replacing them. `.claude/settings.json` still doesn't exist — unchanged,
  out of scope.
- Added the first `.claude/settings.json` safety baseline (blueprint §9.1
  Block 4 / §10): `defaultMode: "plan"`, `disableBypassPermissionsMode` and
  `disableAutoMode` both set to `"disable"`, a hard `deny` list covering
  `.env`/secrets/credentials/tokens, broker/account/order/pnl/payment/
  banking (singular and plural forms), raw-data and export-staging paths
  (`raw/**`, `**/raw-transcripts/**`, `**/raw-screenshots/**`,
  `**/raw-clips/**`, `**/raw-uploads/**`, `transcripts/raw/**`,
  `screenshots/raw/**`, `artifacts/raw/**`, `**/account-exports/**`,
  `**/order-exports/**`), the not-yet-existing `07_Research-Library/_inbox/**`
  staging path, `live-account/**`, `.claude/settings.local.json`, and
  `.claude/worktrees/**` — each denied for `Read`/`Edit`/`Write`, not just
  `Read`. Also denies `mcp__*` outright (no MCP server is configured; this
  blocks any future one by default) and the git-destructive/broad-dangerous
  Bash patterns from blueprint §10 (force-push, `reset --hard`, `rm -rf`,
  broker/order/buy/sell keywords). `Write`/`Edit` and all git-mutating
  commands are `ask`; only read-only git inspection
  (`status`/`diff`/`log`/`show`) and bare `Read` are pre-allowed. `hooks: {}`
  stays empty — no hooks, no MCP config, no routines, no scripts, no
  Dispatch. Not yet live-tested inside a running session; see
  `CURRENT_STATE.md`.
- Addressed automated PR review (Codex) on the settings baseline: added the
  missing `live-account/**` deny triple (already in `.gitignore` and
  blueprint §10, dropped by oversight); broadened the force-push deny from
  `Bash(git push --force*)`/`Bash(git push -f*)` (which only match when the
  flag is the first argument) to also match `--force`/`-f` appearing later
  in the command, e.g. `git push origin main --force`. Removed
  `Bash(curl *|*)`/`Bash(irm *|*)`/`Bash(iwr *|*)` — Claude Code matches
  compound Bash commands per-subcommand (split on shell separators like
  `|`), so a rule containing a literal `|` never matches either subcommand
  and these denies were dead code, not an actual hard block. Pipe-to-shell
  now correctly falls to the default `ask` gate (`defaultMode: "plan"`
  already requires human review of every unlisted Bash command) rather than
  being falsely advertised as denied outright.
- Added the first `.claude/rules/**` behavioral baseline: six unconditional
  rule files (`entrylens-trading-safety.md`, `alexander-aios-boundary.md`,
  `research-source-quality.md`, `repo-hygiene.md`,
  `decision-ledger-discipline.md`, `claude-surface-routing.md`) plus a
  `README.md` index. These are short, load-on-relevance reference rules —
  one rung below skills on the automation ladder, not wired into any
  auto-load/hook mechanism. `entrylens-trading-safety.md` carries the
  verbatim Green definition and the full trading-safety boundary
  unconditionally (not path-scoped), matching blueprint §1's "highest
  authority" status. `decision-ledger-discipline.md` introduces a
  proposed/accepted/superseded status discipline for future decision
  logs — it does not modify `Templates/decision-log.md` or
  `00_System/Decision-Index.md`, which still have no `status` field; that
  remains a follow-up if the discipline is adopted.
- Authored the first decision log,
  `08_Decision-Logs/ADR-0001-trader-intelligence-company.md` (status:
  accepted, owner: bigmannot23): records the strategic doctrine that Claude
  becomes a trader-intelligence company around EntryLens and does not trade —
  fixing the intent of the ingestion system before any ingestion skill is
  built. The ADR enumerates what Claude is allowed to do (research digestion,
  product/QA/compliance support, source-quality review, claim auditing,
  candidate predicates/rules and candidate replay fixtures for human review,
  decision logs) and what it must never become (trader, signal engine,
  scanner, broker/account/order/P&L/payment/banking surface, market
  automation agent, trade-recommendation system, EntryLens Green authority),
  quotes the Green definition verbatim, and sets a supersession policy.
  Replaced the placeholder rows in `00_System/Decision-Index.md` and
  `08_Decision-Logs/_index.md` with the real ADR-0001 references and ticked
  the `First decision log entry` box in `CURRENT_STATE.md`. No automation,
  scripts, hooks, or runtime added.
- Authored the second Alexander-AIOS skill, `claim-audit`
  (`.claude/skills/claim-audit/SKILL.md`): a read-first, human-invoked
  classifier that extracts claims from a pasted draft/source note/research
  summary, sorts them into facts/recommendations/assumptions/open
  questions per `.claude/rules/research-source-quality.md`, audits each
  factual/recommendation claim for source name, date, confidence, and
  staleness, and runs an independent safety scan across all buckets for
  trading-adjacent or EntryLens-Green language per
  `.claude/rules/entrylens-trading-safety.md`. Ends with one of four
  verdicts (pass / pass with caveats / block until sourced / block for
  safety) and includes 5 test cases. It performs no ingestion, edits no
  files by default, and never promotes a claim into
  `00_System/Claim-Index.md` itself — that stays a human step. Marked
  `claim-audit` built in `.claude/skills/_index.md` and
  `01_ClaudeOps/Skills/_index.md` (now 2 of 12 first skills built).
- Authored the third Alexander-AIOS skill, `source-quality`
  (`.claude/skills/source-quality/SKILL.md`): a read-first, human-invoked
  classifier that runs on a raw source *before* ingestion or `claim-audit`
  — it records source identity (title, author/provider, date/freshness,
  URL/location), classifies source type (9 named types) and reliability
  (high/medium/low/reject), checks rights/use restriction (cite-use,
  paraphrase only, human-review only, do not ingest, reject), runs an
  independent safety scan (no date, stale source, marketing/hype,
  unverifiable claims, copyright/raw-transcript risk, trading-advice risk,
  broker/account/P&L/payment/banking risk, AI-authorized Green risk), and
  recommends one of six next steps. Ends with one of five verdicts (accept
  for claim-audit / accept with restrictions / human-review only / reject
  / block for safety) and includes 5 test cases. It is a deliberately
  lighter, earlier, qualitative gate than the existing 0–20
  `00_System/Source-Quality-Rubric.md` score — it does not compute that
  number itself, and a source can still need the numeric rubric and the
  `Rights-IP-Gate.md` green/amber/red check independently once it clears
  this gate. The skill draws a hard line between a source merely being
  restricted-use (paraphrase only — recorded in Rights/use restriction,
  not a safety flag) and a source where raw transcript/media storage,
  reproduction, or scraping is actually requested or implied (a genuine
  `copyright/raw-transcript risk` safety flag) — conflating the two would
  have wrongly blocked legitimately accessed, restricted-use sources.
  References `.claude/rules/research-source-quality.md`,
  `.claude/rules/entrylens-trading-safety.md`, and ADR-0001 as source of
  truth, and names `claim-audit` as the next gate for factual claims. It
  performs no ingestion, writes no source notes, stores no raw
  transcript/media, edits no files by default, and never computes or
  authorizes EntryLens Green. Edited row 5 of
  `01_ClaudeOps/Skills/_index.md` in place (previously
  `source-quality-score`, Proposed) to point at the built skill rather
  than adding a duplicate row, since the 0–20 numeric rubric it described
  remains a separate, still-manual step in `Source-Quality-Rubric.md`.
  Marked `source-quality` built in `.claude/skills/_index.md` (now 3 of 12
  first skills built).
- Authored the fourth Alexander-AIOS skill, `research-ingest`
  (`.claude/skills/research-ingest/SKILL.md`): a read-first, human-invoked
  transformer that turns a source already cleared by `source-quality` into
  a sanitized draft research note. It requires a prior `source-quality`
  verdict as input — it stops with **Needs source-quality first** if none
  is supplied, and with **Blocked by source-quality** or **Blocked for
  trading safety** if that verdict is Reject, Human-review only, or Block
  for safety, with one narrow exception: if the user explicitly asks only
  for a short cautionary/rejection note recording why a blocked source
  wasn't used, it produces that note alone (header, gate inputs, and a
  Rejected/no-use items entry) without extracting any other content, and
  still ends in a blocked verdict. For sources that clear the gate, it
  extracts content into ordered, always-present buckets (research ingest
  header, gate inputs, source summary, supported claims, unsupported/
  needs-source claims, useful patterns, risks and caveats, EntryLens
  candidate material, ClaudeOS/AIOS upgrade candidates, decision-log
  candidates, rejected/no-use items, open questions, verdict), running
  `claim-audit` against every extracted claim before any claim can be
  marked "supported" — if claim-audit hasn't run yet, it stops with
  **Needs claim-audit first** rather than guessing. Every EntryLens-
  adjacent item (candidate predicates, candidate replay-fixture ideas,
  product hypotheses) is extracted only under a `HUMAN-REVIEW ONLY:`
  prefix per ADR-0001's "staged proposals only, never self-authorized"
  boundary, and anything that would require trade-recommendation framing
  to state is dropped to Rejected/no-use instead of being laundered
  through "candidate" language. It never stores raw transcript/
  screenshot/clip/upload/audio/video/copyrighted material regardless of
  source-quality use permission, never writes to canonical doctrine or
  product files (`Master-Blueprint-V1.md`, `CLAUDE.md`,
  `OPERATING_DOCTRINE.md`, `.claude/rules/**`, `03_EntryLens/**`,
  `04_AlphaLab/**`, `Claim-Index.md`, `Decision-Index.md`), never
  promotes a candidate predicate/rule/replay-fixture/hypothesis into
  implementation, never recommends trades/setups/entries/exits/contracts/
  sizing/stops/targets/probability/edge/win-rate/expected-return, and
  never computes or authorizes EntryLens Green. References
  `.claude/rules/research-source-quality.md`,
  `.claude/rules/entrylens-trading-safety.md`, ADR-0001, and both
  `source-quality` and `claim-audit` as upstream gates. Ends with one of
  six verdicts (ready for human review / needs source-quality first /
  needs claim-audit first / blocked by source-quality / blocked for
  trading safety / rejected — no useful action) and includes 5 test
  cases. Added a row to `.claude/skills/_index.md` and appended row 13 to
  `01_ClaudeOps/Skills/_index.md`'s first-skills table (now 4 of 12 first
  skills built) — `youtube-ingest`'s pre-reserved row 2 is untouched,
  since it remains a narrower, source-type-specific skill distinct from
  this general-purpose one.
