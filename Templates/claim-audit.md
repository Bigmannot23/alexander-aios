# Template — Claim Audit

Required for any factual, legal, financial, or performance claim before it
enters source-of-truth anywhere in this repo. Log the result as a row in
`00_System/Claim-Index.md` and keep this full form with the source's
packet/lesson.

```yaml
claim_id:
date:
raw_claim:              # exact original wording, for reference only
```

**Transformed claim** (never adopt raw trading advice verbatim — see
`Master-Blueprint-V1.md` §7.4 transformation table):

**Confidence level:** low | medium | high

**Sources for:**

**Sources against:**

**Use restriction** (e.g. "research only," "requires independent
verification," "rejected — prohibited"):

## Trading-claim transformation reference

| Raw input | Safe brain output |
|---|---|
| "Take calls when X" | Setup concept + historical-review hypothesis |
| "This wins 80%" | Unverified claim requiring independent audit |
| "Enter on breakout confirm" | Predicate candidate: observable breakout definition + volume condition + invalidation |
| "Avoid chop" | No-trade filter candidate with observable conditions |
| "Use this contract" / "Buy now" | Rejected — prohibited |
