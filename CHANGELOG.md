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
