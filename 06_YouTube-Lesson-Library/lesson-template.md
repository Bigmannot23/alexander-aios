---
read_first: _index.md
source_of_truth: Master-Blueprint-V1.md §7.5
---

# Lesson Template (YouTube)

## What belongs here

The YouTube-specific variant of `Templates/lesson.md` — adds video
metadata and timestamp citation fields that a generic lesson doesn't need.

## What does not belong here

The generic, source-agnostic lesson template (see `Templates/lesson.md`,
used by Course Library and Research Library too).

## Read-first file

`_index.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §7.5 (ingest output schema).

---

## Template

```yaml
source: youtube
video_title:
video_creator:
video_url:           # for your own reference only; never re-published raw
published_date:
ingested_date:
rights_status:        # green | amber | red — see 00_System/Rights-IP-Gate.md
quality_score:         # 0-20 — see 00_System/Source-Quality-Rubric.md
input_type:           # transcript | ai-summary | hybrid | first-hand
summary_provenance:   # tool/model that produced the summary, or n/a
input_trust:          # primary | secondary-unverified (ai-summary is always secondary-unverified)
```

**Extracted concepts** (definitions/mechanics only):

**Claims requiring audit** (link each to `00_System/Claim-Index.md`):

**Implementation lessons** (what changes in how I operate, not what trade
to take):

**Productization candidates** (if any — link to
`02_Productization-Lab/_index.md`):

**Trading/education candidates** (deterministic only — setup concepts go
to `04_AlphaLab/Setups/_index.md`, filters to
`04_AlphaLab/No-Trade-Filters/_index.md`):

**Promotion candidates** (if any — see `promotion-rules.md` in this
folder):

**Rejected ideas** (and why):

**Proof required** (what would need to be true to trust this):
