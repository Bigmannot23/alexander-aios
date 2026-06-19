---
read_first: CLAUDE.md
source_of_truth: 08_Decision-Logs/_index.md; 00_System/Decision-Index.md; Master-Blueprint-V1.md Appendix B
scope: unconditional
---

# Decision Ledger Discipline

- Every durable decision (changes doctrine, architecture, or a system
  boundary) gets a decision log in `08_Decision-Logs/` (via
  `Templates/decision-log.md`) and a row in `00_System/Decision-Index.md`.
- Track status explicitly: proposed → accepted → superseded. Don't leave a
  decision's status implicit.
- Never silently rewrite an accepted decision.
- If a decision changes, write a new entry that supersedes the old one and
  link both directions. The old entry stays in the ledger, marked
  superseded — it's history, not an error to erase.

Full reasoning: `08_Decision-Logs/_index.md`, `00_System/Decision-Index.md`,
`Master-Blueprint-V1.md` Appendix B.
