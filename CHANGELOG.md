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
