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
  `01_ClaudeOps/Skills/_index.md`'s first-skills table (now 4 first skills
  built) — `youtube-ingest`'s pre-reserved row 2 is untouched,
  since it remains a narrower, source-type-specific skill distinct from
  this general-purpose one.

## 2026-06-20

- Ran `route-task` on the incoming task before authoring anything, per
  the routing skill's own procedure: classified as System =
  Alexander-AIOS brain/ClaudeOps, Work class = Immediate, Safety class =
  Research only, Claude surface = Claude Code, model = Sonnet, with no
  hard-stop rule firing — confirming the task was clear to proceed before
  any file was touched.
- Authored the fifth Alexander-AIOS skill, `youtube-ingest`
  (`.claude/skills/youtube-ingest/SKILL.md`): the video/course/podcast/
  transcript-specific specialization of `research-ingest`, stricter about
  timestamps, transcript handling, and copyright because video/course
  sources carry materially higher reproduction risk than a generic
  article or paper. Like `research-ingest`, it requires a prior
  source-quality verdict as input — stopping with **Needs source-quality
  first** if none is supplied, and with **Blocked by source-quality** or
  **Blocked for trading safety** if that verdict is Reject, Block for
  safety, or Human-review only, with the same narrow exception for a
  short cautionary/rejection note. It treats video/course/podcast/
  transcript sources as **restricted-use-by-default** unless rights are
  clearly permissive, recording that determination explicitly in a
  dedicated Rights/use restriction section — a stricter default posture
  than `research-ingest` takes, reflecting the higher copyright exposure
  of video/course material. Timestamp references are extracted only as
  paraphrased concept pointers (e.g. "~12:30 — creator explains a
  breakout-confirmation concept"), never as transcript reproduction, with
  quoting limited to brief fair-use-defensible snippets regardless of the
  rights determination. Every factual/performance claim is gated through
  `claim-audit` before being marked supported — if claim-audit hasn't run,
  is unavailable, or errors, the skill stops with **Needs claim-audit
  first** rather than guessing. A request-side hard stop, independent of
  the source's own content, fires immediately if the user's own request —
  not the source — asks for a trade recommendation/signal or asks Claude
  to authorize or compute EntryLens Green, citing
  `.claude/rules/entrylens-trading-safety.md` and reprinting the Green
  definition verbatim where relevant. Every EntryLens-adjacent item
  (candidate predicates, candidate rules, replay-fixture ideas, product
  hypotheses) is extracted only under a `HUMAN-REVIEW ONLY:` prefix, per
  ADR-0001's "staged proposals only, never self-authorized" boundary. It
  never stores raw transcript, copied course text, video/audio, clips,
  screenshots, or uploads regardless of the rights determination, never
  writes to canonical doctrine or product files
  (`Master-Blueprint-V1.md`, `CLAUDE.md`, `OPERATING_DOCTRINE.md`,
  `.claude/rules/**`, `03_EntryLens/**`, `04_AlphaLab/**`,
  `00_System/Claim-Index.md`, `00_System/Decision-Index.md`) or any
  `06_YouTube-Lesson-Library/**` pipeline folder (`inbox/`,
  `raw-transcripts/`, `packets/`, `lessons/`, `promoted/**`), never
  promotes a candidate predicate/rule/replay-fixture/hypothesis into
  implementation, never recommends trades/setups/entries/exits/contracts/
  sizing/stops/targets/probability/edge/win-rate/expected-return, and
  never computes or authorizes EntryLens Green. The skill's own "Source of
  truth" section explicitly disambiguates it from the pre-existing,
  different artifact `Templates/youtube-ingest.md` — a manual,
  rights-gated, rubric-scored 10-step checklist that drives the real
  `06_YouTube-Lesson-Library` pipeline (raw-transcripts → packets →
  lessons → promotion). `youtube-ingest` the skill is a separate, lighter,
  standalone advisory layer that produces a single human-review note and
  never writes into that pipeline or replaces the template's rights/rubric
  mechanics — it cites the template and `transcript-policy.md` rather than
  duplicating or superseding them. Ends with one of six verdicts (ready
  for human review / needs source-quality first / needs claim-audit first
  / blocked by source-quality / blocked for trading safety / rejected — no
  useful action) and includes 5 test cases (using lettered sub-cases to
  cover all 9 required hard-stop triggers without exceeding the 5-case
  ceiling, the same compression technique `source-quality`'s boundary-
  contrast case already uses). Used `source-quality/SKILL.md`'s
  single-quoted YAML frontmatter form rather than research-ingest's bare
  form, since `git log` confirms this repo previously hit a real invalid-
  YAML-frontmatter bug on an unquoted multi-clause description (fixed in
  commit `0f66094`). Added a row to `.claude/skills/_index.md` and flipped
  row 2 of `01_ClaudeOps/Skills/_index.md`'s first-skills table from
  Proposed to Built — that row was pre-reserved for this skill by the
  blueprint's own §8.1 table, so this is a flip in place rather than a new
  row appended — and corrected its Output column, which had described
  `Templates/youtube-ingest.md`'s pipeline output rather than this
  skill's actual output. Now 5 first skills built; header and footer
  counts in both index files updated to match.
- PR #16 (this skill) drew a GitHub Copilot review with 5 inline findings.
  An automated auto-fix process pushed partial fixes for all 5 directly to
  the branch before this session's own fixes landed, and the PR merged
  before either competed. Re-checked all 5 against the merged `main`: 3
  (the missing "podcast" in `01_ClaudeOps/Skills/_index.md`'s Trigger
  column, the output schema's claim-audit status values, and the missing
  `00_System/` prefix on the Claim-Index/Decision-Index paths in this
  entry) were already fixed correctly and needed no further action. 2 were
  only superficially patched — the direct-request hard stop (originally
  step 9) got a parenthetical "(evaluate immediately after step 2)" note
  instead of an actual reorder, still leaving every extraction step
  textually before the safety check; and the rights/use-restriction step's
  false claim about changing step 6's quoting limit was removed without
  restoring any link to the output schema's Citable scope field, leaving
  it dangling. Fixed both directly on `claude/amazing-cerf-esuchp`: moved
  the direct-request hard stop to step 1 and renumbered the procedure 1-15
  so the safety check actually runs first, and rewrote the rights/use-
  restriction step to set the Citable scope field while keeping the
  timestamped-concepts quoting limit and the never-store-raw rule
  unconditional. Updated every downstream step-number cross-reference in
  Verdict precedence and Test cases to match. These fixes are delivered
  via follow-up PR #17, opened after the user explicitly asked for one —
  not as a direct push to the already-merged PR #16.
- PR #17 itself then drew a GitHub Copilot review (Request changes) with
  2 findings: step 1's hard stop only covered trade-advice/Green requests,
  not the raw-transcript-storage, transcript-reproduction, or media-
  download requests the PR description also scoped in; and this
  changelog's own "No new PR opened for this follow-up" sentence had gone
  stale the moment PR #17 was opened. Fixed both on
  `claude/amazing-cerf-esuchp`: step 1 now also hard-stops, before any
  extraction, on requests to store raw material, reproduce a transcript
  beyond a brief fair-use-defensible snippet, or download/fetch media —
  citing `transcript-policy.md` and the step-14 never-store-raw rule
  rather than the trading-safety rule used for the trade-advice/Green
  case — and test case 4(a)-(c) updated to reflect that these are now
  step-1 enforcement, not just generic refusals. The stale changelog
  sentence above was rewritten to name PR #17 directly.
- Ran `route-task` on the incoming task before authoring anything, per the
  routing skill's own procedure: classified as System = ClaudeOps/AIOS,
  Work class = V1 core, Safety class = Safe live cognitive support, Claude
  surface = Claude Code, model = Opus, with no hard-stop rule firing —
  this is non-advisory meta-safety tooling, clear to proceed.
- Authored the sixth Alexander-AIOS skill, `trading-safety-review`
  (`.claude/skills/trading-safety-review/SKILL.md`): a read-first,
  human-invoked (or invoked by a human acting on a `route-task`
  recommendation when an artifact is trading-adjacent) safety reviewer that audits any artifact — text,
  plans, skills, research notes, product ideas, PR diffs, content drafts —
  line/item by line/item for EntryLens trading-safety drift. Where
  `claim-audit` and `source-quality` each carry a built-in side-check
  across 6 categories with block/pass outcomes, this skill is the
  standalone, line-level, fix-suggesting reviewer those lack: it scans the
  full 14 risk categories (trade recommendations; buy/sell; calls/puts
  recommendations; entry/exit instructions; contracts/strikes/expirations;
  sizing/stops/targets; probability/edge/win-rate/expected-return claims;
  market-scanner behavior; alert/signal behavior; broker/account/order/
  P&L/payment/banking surfaces; AI-trading-assistant behavior; Claude-
  authorized EntryLens Green; Green redefined as buy/sell/edge/probability/
  approval; user-personalized securities advice), classifies each finding
  into one of five distinctions (safe plan-alignment, human-review
  candidate, unsafe recommendation/signal, product-scope drift, legal/
  compliance caution), assigns one of four severities (pass / caution /
  request changes / block), and ends with one of four verdicts (Pass /
  Pass with cautions / Request changes / Block). A quote-context gate runs
  before any severity is judged so that unsafe language quoted *only as
  something to reject* (negative examples, test fixtures, a rules file's
  own prohibition list, this skill quoting itself) is flagged
  present-but-safe and never fires the hard-stop — without it the reviewer
  would falsely Block the rules files, ADR-0001, the sibling skills, and
  itself. It reprints the canonical Green definition verbatim whenever
  Green is referenced, supplies exact safe replacement wording (before →
  after) for every unsafe finding, and on product-scope drift names
  `product-scope-review` as the next/companion skill while noting it is not
  built yet. The skill is itself non-advisory: it never validates or finds
  trades, recommends setups, computes edge, or authorizes/computes
  EntryLens Green; it edits no files by default, performs no ingestion,
  writes no source notes, stores no raw material, and never becomes a
  script/hook/routine/Dispatch workflow. References
  `.claude/rules/entrylens-trading-safety.md`,
  `.claude/rules/research-source-quality.md`,
  `00_System/Master-Blueprint-V1.md` §1/§6, `00_System/Safety-Policy.md`,
  and ADR-0001. Includes 5 test cases (clean plan-alignment → Pass; legal/
  compliance caution → Pass with cautions; scanner/alert product-scope
  drift → Request changes; live hard-stop breach → Block; quoted-to-reject
  exception → Pass). Added a row to `.claude/skills/_index.md` and appended
  row 14 to `01_ClaudeOps/Skills/_index.md`'s first-skills table, bumping
  the built count from 5 to 6.
