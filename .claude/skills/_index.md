---
read_first: CLAUDE.md
source_of_truth: 01_ClaudeOps/Skills/_index.md
---

# Claude Code Skills Index

## What belongs here

The actual Claude Code skill definitions for this repo — one `SKILL.md`
per subfolder, named after the skill.

## What does not belong here

Doctrine-side tracking of skill proposals and promotion stage (system,
trigger, output, work class, safety class) — that lives in
`01_ClaudeOps/Skills/_index.md`. This file only indexes what is actually
implemented under `.claude/skills/`.

## Read-first file

`CLAUDE.md`, then `01_ClaudeOps/Skills/_index.md`.

## Built skills

| Skill | Path | Class | Status |
|---|---|---|---|
| route-task | `.claude/skills/route-task/SKILL.md` | Immediate | Built |
| claim-audit | `.claude/skills/claim-audit/SKILL.md` | V1 core | Built |
| source-quality | `.claude/skills/source-quality/SKILL.md` | V1 core | Built |
| research-ingest | `.claude/skills/research-ingest/SKILL.md` | V1 core | Built |
