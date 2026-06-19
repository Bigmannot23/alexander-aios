---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §7.2
---

# Rights / IP Gate

## What belongs here

The gate every source must pass *before* ingest — separate from and prior
to source-quality scoring.

## What does not belong here

Quality assessment (see `Source-Quality-Rubric.md`) and claim verification
(see `Claim-Index.md`). A source can be rights-green and quality-terrible,
or rights-red and quality-excellent — the gates are independent.

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §7.2. This file expands that section; the
blueprint wins on conflict.

---

## The rule

Score each source **green / amber / red** before ingest:

| Status | Meaning |
|---|---|
| **Green** | Can be stored privately as transformed notes; excerpts may be retained with attribution; safe to reference internally without restriction. |
| **Amber** | Can be stored privately as transformed notes only; no verbatim excerpts beyond fair-use-defensible quotation; document the specific restriction in the source's packet. |
| **Red** | Do not ingest. Do not store transformed notes either. Log the rejection and move on. |

## Hard rules

- Store **transformed private notes only** — never a raw paid-course or
  video archive. Raw transcripts are gitignored and local-only
  (`**/raw-transcripts/` in `.gitignore`).
- Attribution alone does not make reuse safe. Fair use is case-by-case, not
  a default assumption.
- Commercial reuse (e.g. a Productization Lab output, or public-facing
  EntryLens copy) requires a **separate, stricter** check than internal
  private notes — green for internal use does not imply green for resale.
  When in doubt, treat as amber and restrict to internal-only.

## Where this is used

`Templates/course-ingest.md` and `Templates/youtube-ingest.md` both run the
rights gate before anything else. A red verdict stops the pipeline at that
template's first step.
