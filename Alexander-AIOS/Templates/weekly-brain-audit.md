# Template — Weekly Brain Audit

The `weekly-brain-audit` skill (proposed, not yet built — see
`01_ClaudeOps/Skills/_index.md` #12) produces this. Run weekly.

```yaml
week_of:
```

**Decay check** — any doc whose claims/links no longer match reality:

**Stale check** — any `_index.md` whose "Status" section is outdated:

**Missing-proof check** — any runtime-sensitive change without a matching
proof log (`00_System/Proof-Index.md`):

**Unprocessed staging count** — items sitting in any inbox/staging folder
(target: ≤20 per `Master-Blueprint-V1.md` §9.2):

**Current-state freshness** — days since `CURRENT_STATE.md` was last
updated:

**Flags for this week:**

**Nothing unsafe found:** yes | no — if no, stop and escalate before
continuing any automation work.
