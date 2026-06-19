# Routines — deferred

Routines are deferred until scripts pass 5–10 clean manual runs.

No routines exist in this repo. None will be created until the underlying
scripts in `01_ClaudeOps/Scripts/_index.md` have accumulated 5–10 clean
manual runs and proof/log behavior is trusted. See `Master-Blueprint-V1.md`
§12 (automation ladder) and §15 (do-not-build-yet list).

Even after that gate is passed, routines are scoped to **unattended
read-only repo maintenance only** (e.g. doc-drift checks, nightly index
refresh, weekly drift-audit drafts for human review). **Any trading/
market/account/live-decisioning routine is a hard deny, permanently** —
see `Master-Blueprint-V1.md` §11 and `00_System/Safety-Policy.md`.
