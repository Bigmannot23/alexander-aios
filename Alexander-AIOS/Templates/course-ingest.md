# Template — Course Ingest

Copy this into `05_Course-Library/_index.md`'s eventual storage location (or
a dated file alongside it) for each course source.

```yaml
source: course
course_title:
provider:
creator:
date_purchased_or_accessed:
ingested_date:
rights_status:        # green | amber | red — 00_System/Rights-IP-Gate.md
quality_score:          # 0-20 — 00_System/Source-Quality-Rubric.md
notes:
```

1. **Rights/IP gate** (`00_System/Rights-IP-Gate.md`) — run first. Red stops
   here.
2. **Research packet** — raw notes, transformed, never a course-content
   dump.
3. **Lesson file** — use `Templates/lesson.md`.
4. **Claim audit** — every factual/performance claim, via
   `Templates/claim-audit.md`, logged to `00_System/Claim-Index.md`.
5. **Implementation map** — what this changes about how I operate.
6. **Promotion candidate** (if any) — `Templates/promotion.md`.
7. **Rejected ideas** — and why.
8. **Next action.**
