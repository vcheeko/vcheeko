# KORA — External Technical Review Packet

**Purpose:** give an independent reviewer a compact, falsifiable surface for reviewing KORA without exposing private implementation or credentials.

## 1. What KORA claims today

KORA is a prototype / evidence-building operating layer for human-directed AI-assisted work. Its core thesis is that serious AI workflows need explicit state, dependencies, authority boundaries, bounded execution, evidence, verification and recovery.

KORA is **not** claimed to be production-ready, independently validated, or publicly reproduced end to end.

## 2. What to review first

Recommended order:

1. [`KORA_PUBLIC_SHOWCASE.md`](KORA_PUBLIC_SHOWCASE.md)
2. [`KORA_GOLDEN_DEMO_SCORECARD.md`](KORA_GOLDEN_DEMO_SCORECARD.md)
3. [`KORA_EVIDENCE_INDEX.md`](KORA_EVIDENCE_INDEX.md)
4. [`public-evidence/kora-golden-v0.2/`](public-evidence/kora-golden-v0.2/)
5. [`KORA_PUBLIC_GOLDEN_DEMO_ARCHITECTURE.md`](KORA_PUBLIC_GOLDEN_DEMO_ARCHITECTURE.md)

## 3. Questions the review should try to falsify

Please challenge these rather than assuming them true:

### Architecture

- Is the separation between human intent, authorization, execution and verification meaningful or mostly documentation?
- Are the lifecycle states sufficient to distinguish requested, executed and verified work?
- Could conflicting or stale state silently become canonical?
- Does the proposed recovery model avoid accidental duplicate execution?

### Authority and safety

- Are high-consequence actions clearly separable from low-risk automatic work?
- Are fail-closed paths explicit enough?
- Is least privilege a structural property or merely a stated intention?
- Where could prompt/model output gain more authority than intended?

### Evidence

- Does the public verifier actually bind evidence to a terminal claim?
- Can malformed, missing, duplicated or forged evidence pass unexpectedly?
- Are provenance claims meaningful enough to support the conclusions drawn from them?
- Are any public claims stronger than the reproducible evidence?

### Public Golden Demo target

- Is the proposed public runtime small enough to audit?
- Does it demonstrate KORA's distinctive value rather than only a conventional test harness?
- Which negative/recovery scenario is missing?
- What is the smallest additional test that would materially increase confidence?

## 4. Requested reviewer output

A useful review can be short. Please return:

```text
REVIEWER:
DATE:
COMMIT / VERSION REVIEWED:

VERDICT:
[ ] credible within stated scope
[ ] credible with required changes
[ ] insufficient evidence
[ ] architecture concern

TOP 3 STRENGTHS:
1.
2.
3.

TOP 3 RISKS / GAPS:
1.
2.
3.

ONE FAILURE CASE KORA MUST ADD:

ONE CLAIM THAT SHOULD BE NARROWED (if any):

ONE NEXT TEST THAT WOULD MOST INCREASE CONFIDENCE:

PUBLIC REPRODUCTION ATTEMPT:
[ ] pass
[ ] fail
[ ] not attempted

NOTES:
```

## 5. Scope boundary

No reviewer is asked to endorse KORA as secure, production-ready or commercially validated based on this public packet.

A positive review should mean only that the reviewed architecture/evidence is credible **within the explicitly stated prototype scope**.

## 6. Founder / AI disclosure

KORA is developed with extensive AI assistance. Product direction, acceptance criteria and consequential project decisions remain human-owned; AI output is treated as proposed work that requires evidence or review proportional to consequence.

## 7. Preferred review style

The most valuable review is adversarial but specific: identify a concrete way the model, worker, verifier, persistence layer or public evidence could produce a false completion claim, unsafe authority escalation or misleading confidence signal.

**Evidence before scale. Critique before confidence.**
