---
read_first: CLAUDE.md
source_of_truth: Master-Blueprint-V1.md §9.1, §15
---

# Dashboards Index

## What belongs here

The maturity model (0–50) and KPI checklist for the whole repo — kept as a
markdown checklist, by explicit doctrine, not an app.

## What does not belong here

Any actual dashboard application, auto-generated report, or live data
connection. `Master-Blueprint-V1.md` §15 explicitly lists "Dashboards as a
build project" in the do-not-build-yet list: "keep the maturity model a
checklist."

## Read-first file

`CLAUDE.md`, then this file.

## Source-of-truth files

`Master-Blueprint-V1.md` §9.1 (work block 7: "score maturity 0–50") and
§15.

---

## Maturity checklist (0–50)

See `CURRENT_STATE.md` for the live, up-to-date checklist — duplicating it
here would create two sources of truth that drift. This index exists so
`10_Dashboards/_index.md` is discoverable from the routing map; the actual
checklist lives in `CURRENT_STATE.md` since that file is already the
designated living-state document.

## KPI list (to track once systems are running)

- Fixtures count (↑ is good)
- Repeated-mistake rate (↓ is good)
- Repeated prompts converted to skills (↑ is good)
- Decisions logged vs. re-litigated (logged ratio ↑ is good)
- Current-state freshness (days since last update, ↓ is good)
- "Can a cold session recover without me" (qualitative, periodic check)
