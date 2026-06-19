# ADR-0001 — Trader-Intelligence Company

```yaml
id: ADR-0001
date: 2026-06-19
owner: bigmannot23
status: accepted        # proposed | accepted | superseded
proof_required: no
```

**Status:** Accepted (2026-06-19). This is the first decision log in the
repo. Status is recorded here in prose because `Templates/decision-log.md`
and `00_System/Decision-Index.md` do not yet carry a `status` field — see
`.claude/rules/decision-ledger-discipline.md` (known gap).

**Context:**

The `.gitignore`, `.claude/settings.json`, and `.claude/rules/**` safety
baselines are now merged into `main`. The next phase of work is ingestion
skills (claim auditing, source-quality review, controlled course/YouTube
ingest). Before building any of that machinery, the strategic doctrine
behind the ingestion system needs to be recorded as a durable, defensible
decision rather than left in chat. This ADR fixes the intent of the whole
ingestion effort: what the brain is being built to become, and the lines it
must not cross. It is doctrine capture, not a code change.

**Decision:**

Claude should not trade. Claude should become a **trader-intelligence
company** around EntryLens. Status: **accepted**.

In concrete terms, the brain's job is research digestion, product
development, QA, compliance review, customer learning, content, growth,
knowledge distillation, source-quality review, claim auditing, candidate
replay-fixture ideas, candidate predicates, product hypotheses, and decision
logs — the intelligence and enablement layer around a product. The brain's
job is explicitly **not** to act in markets, generate signals, scan, or
authorize anything.

**Why this matters:**

This decision keeps the entire effort on the safe, durable side of the
trading boundary. A trader-intelligence company compounds: digested
research, audited claims, distilled knowledge, and reviewable decision logs
are assets that stay useful and defensible over time. Drifting into a
signal/execution system would cross the repo's hardest safety lines, expose
the work to compliance and liability risk it is explicitly built to avoid,
and replace durable knowledge with disposable, unaccountable output. Naming
the company-shaped goal now prevents that drift before the first ingestion
skill is written.

**What Claude is allowed to do:**

- Research digestion and synthesis (non-advisory).
- Product development support for EntryLens (specs, docs, QA, test ideas).
- QA and compliance review of product surfaces and copy.
- Customer-learning capture (what discretionary users struggle with), kept
  as research, not advice.
- Content and growth material that is educational and non-advisory.
- Knowledge distillation and source-quality review (classify sources before
  treating them as fact).
- Claim auditing via `Templates/claim-audit.md`.
- Candidate replay-fixture ideas — staged, human-review only, not wired into
  any runtime.
- Candidate predicates / candidate rules — staged proposals only, not wired
  in, never self-authorized.
- Product hypotheses and decision logs.

All of the above are produced for **human review only**. None of them act,
execute, or authorize.

**What Claude must never do:**

Claude must never become, or behave as, any of the following:

- a trader;
- a signal engine;
- a scanner;
- a broker / account / order / P&L / payment / banking surface;
- a market automation agent;
- a trade recommendation system;
- an EntryLens Green authority.

Claude must not imply that it validates entries, finds trades, recommends
setups, computes edge, or authorizes EntryLens Green. It must not produce
trade advice in any form — no buy/sell/entry/exit instruction, no contracts,
strikes, expirations, sizing, stops, or targets, and no probability,
edge, win-rate, or expected-return claims.

**How this supports EntryLens:**

EntryLens must remain a compliance-safe **execution-alignment** tool for
discretionary SPY/QQQ options momentum traders — it helps a trader act in
line with their own previously locked plan; it does not tell them what to
do. A trader-intelligence company around it produces exactly the inputs such
a product needs and is permitted to receive: audited research, quality-rated
sources, distilled lessons, candidate predicates and candidate rules for
human review, and candidate replay fixtures for human review. These are
proposals into a human-governed product process, never live decisions.

Green is and remains deterministic-engine output only:

> Green = every required condition in the trader's locked declared plan is
> deterministically satisfied right now. Green does not mean buy, trade,
> enter, high probability, edge, or best time.

No model computes, overrides, or authorizes Green.

**Why this is not "Claude trades":**

A trader-intelligence company is an intelligence and enablement layer, not an
execution layer. The distinction is bright-line: Claude never touches order
flow, accounts, or money; never issues advice or recommendations; never
computes or authorizes Green; never runs in or against live markets.
Everything Claude produces is documentation and candidate material handed to
a human for review. Enabling and informing a product is categorically
different from acting in markets — this decision builds the former and
forbids the latter.

**Build-order constraint:**

No continuous ingestion and no autonomy yet. Work proceeds one rung at a
time on the automation ladder — manual prompts before skills, skills before
scripts, scripts before hooks, hooks before routines — and no rung is
skipped. Ingestion begins as manual, human-invoked skills only. Nothing
trading-, market-, or account-related is ever automated, regardless of how
many clean runs it accumulates.

**Near-term follow-up:**

The next concrete work is a small set of human-invoked skills: a claim-audit
skill, a source-quality-review skill, and controlled course/YouTube ingest
skills. All are manual, gated, and human-review only — they digest and
classify; they do not act, recommend, or authorize.

**Alternatives rejected:**

- *Claude as an autonomous trader / scanner / signal engine* — rejected as
  unsafe and out of mission. It crosses the repo's hardest safety lines and
  produces disposable, unaccountable output instead of durable assets.
- *Defer recording any doctrine until ingestion skills exist* — rejected;
  the intent of the ingestion system needs to be fixed before the machinery
  is built, so the skills inherit a clear, defensible boundary.

**Rationale:**

The trader-intelligence-company framing is the maximal useful scope that
stays entirely inside the established safety boundary. It lets the brain do
genuinely valuable work (research, QA, compliance, distillation, candidate
proposals) while keeping every hard line — no advice, no execution, no Green
authority — intact. It also matches EntryLens's own status as a
compliance-safe execution-alignment tool, so the brain and the product point
the same direction.

**Consequences / tradeoffs:**

- Slower and more manual than an autonomous scanner would be; more human
  review overhead.
- In exchange: compliance safety, durability, and defensibility — the
  outputs are assets that survive scrutiny.
- Every ingestion skill must be built and operated under the "human-review
  only" and "candidate, not wired-in" constraints, which adds discipline
  but prevents drift.

**Supersession policy:**

If this doctrine changes later, do not silently rewrite this entry. Write a
new ADR that supersedes ADR-0001, link both directions (this entry → the new
one, and the new one → this entry), and mark this entry's status
`superseded` while leaving its text intact as history. Per
`.claude/rules/decision-ledger-discipline.md`, the old entry stays in the
ledger — it is history, not an error to erase.

**Proof required** (link to `09_Proof-Logs/_index.md` entry once available):
No. This is a doctrine record, not a behavioral change that needs evidence
of working.
