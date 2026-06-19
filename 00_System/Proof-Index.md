---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md Appendix B, §1.4
---

# Proof Index

## What belongs here

One row per proof log, repo-wide — evidence that a runtime-sensitive or
otherwise consequential change actually works, written with
`Templates/proof-log.md` and stored in `09_Proof-Logs/_index.md`'s folder.
This is the searchable ledger; the full proof entries live there.

## What does not belong here

The full proof text (diffs, test output, replay evidence). This is a
ledger, not the evidence itself.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` Appendix B (proof-log fields: id · date · change ·
test/replay/metric evidence · diff summary · result · linked decision) and
§1.4 (proof lock: runtime-sensitive changes require tests + proof log +
fixture — no proof, no "done").

---

## Ledger

No proof has been logged yet. Add a row per proof entry using this format:

| ID | Date | Change | Evidence type | Result | Linked decision | Link |
|---|---|---|---|---|---|---|
| _none yet_ | | | | | | |
