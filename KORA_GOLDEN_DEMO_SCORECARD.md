# KORA Golden Demo — Public Scorecard

**Status date:** 2026-08-28  
**Public claim level:** prototype / evidence-building

This scorecard is intentionally conservative. It is designed to prevent private development progress from being presented publicly as a stronger result than the evidence supports.

| Gate | Public status | Meaning |
| --- | --- | --- |
| M1 Contract / precheck | **PRIVATE EVIDENCE: PASS** | Bounded authenticated Golden-task contract and fail-closed precheck exist in private engineering. |
| M2 Bounded execution | **NOT YET CLAIMED** | No public claim of completed bounded execution. |
| M3 Verification / evidence | **NOT YET CLAIMED** | No public claim of complete executor → verifier → VERIFIED transition. |
| M4 Failure / recovery | **NOT YET CLAIMED** | Failure-injection package not yet published. |
| M5 Restart continuity | **NOT YET CLAIMED** | Restart reconstruction not yet published as verified. |
| M6 Sanitized public proof | **IN PROGRESS** | Public evidence contract/scorecard exists; final proof package depends on M2–M5. |

## What M1 currently proves

The reviewed private Golden-task tests include fail-closed checks for a bounded task contract, request authentication/binding, task/state integrity and autonomy budgets. The milestone deliberately leaves execution disabled.

Therefore the correct state is:

```text
CONTRACT/PRECHECK: PASS
EXECUTION: NOT CLAIMED
VERIFICATION: NOT CLAIMED
GOLDEN DEMO v0.1: NOT VERIFIED
```

## Release rule

The scorecard may only move a gate to **PASS** when returned evidence supports it. A dispatched task, implementation commit or successful-looking output is not sufficient.

A public release requires all gates M1–M6 to be PASS.

---

See also: [KORA Engineering Evidence](KORA_ENGINEERING_EVIDENCE.md) and [KORA Public Showcase](KORA_PUBLIC_SHOWCASE.md).
