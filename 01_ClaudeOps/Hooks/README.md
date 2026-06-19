# Hooks — deferred

Hooks are deferred until scripts pass 5–10 clean manual runs.

No hooks exist in this repo. None will be created until a script in
`01_ClaudeOps/Scripts/_index.md` has accumulated 5–10 clean manual runs and
its behavior is trusted. See `Master-Blueprint-V1.md` §12 (automation
ladder) and §15 (do-not-build-yet list).

Even after that gate is passed, no hook may wire anything
trading/market/account-related — that automation is permanently rejected
regardless of how many clean runs a script has (see
`00_System/Safety-Policy.md`).
