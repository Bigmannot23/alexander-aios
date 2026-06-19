# Template — YouTube Ingest

Use alongside `06_YouTube-Lesson-Library/transcript-policy.md`,
`source-quality-rules.md`, `lesson-template.md`, and `promotion-rules.md`.

```yaml
source: youtube
video_title:
creator:
platform: youtube
video_url:
published_date:
ingested_date:
rights_status:        # green | amber | red — 00_System/Rights-IP-Gate.md
quality_score:          # 0-20 — 00_System/Source-Quality-Rubric.md
notes:
```

1. **Rights/IP gate** — run first, on the raw transcript in
   `06_YouTube-Lesson-Library/raw-transcripts/` (gitignored). Red → delete
   raw transcript, log to `06_YouTube-Lesson-Library/rejected/`, stop.
2. **Research packet** → `06_YouTube-Lesson-Library/packets/`.
3. **Lesson file** → `06_YouTube-Lesson-Library/lessons/`, using
   `06_YouTube-Lesson-Library/lesson-template.md`.
4. **Claim audit** — every factual/performance claim, via
   `Templates/claim-audit.md`, logged to `00_System/Claim-Index.md`. For
   trading content, transform per the raw-input → safe-brain-output table
   in `Master-Blueprint-V1.md` §7.4 — never adopt a raw trading claim
   verbatim.
5. **Implementation map** — what this changes about how I operate.
6. **Productization candidates** (if any) →
   `02_Productization-Lab/_index.md`.
7. **Trading/education candidates** (deterministic only) →
   `04_AlphaLab/Setups/_index.md` or `04_AlphaLab/No-Trade-Filters/_index.md`.
8. **Promotion candidate** (if any) →
   `06_YouTube-Lesson-Library/promoted/candidates/`, using
   `Templates/promotion.md`.
9. **Rejected ideas** — and why → `06_YouTube-Lesson-Library/rejected/` or
   `promoted/rejected/`.
10. **Proof required** — what would need to be true to trust this.
