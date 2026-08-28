# KORA Golden Demo — Public Scorecard

**Status date:** 2026-08-28  
**Public claim level:** prototype / evidence-building

This scorecard is intentionally conservative. It separates a **private CI verified vertical slice** from a finished production runtime or a publicly reproducible Golden Demo.

| Gate | Public status | Meaning |
| --- | --- | --- |
| M1 Contract / precheck | **PRIVATE EVIDENCE: PASS** | Bounded authenticated Golden-task contract and fail-closed precheck exist in private engineering. |
| M2 Bounded execution | **PRIVATE CI: BOUNDED-SLICE PASS** | One deterministic allowlisted read-only worker executes within an explicit scope/budget, with replay protection. Production/application executor integration remains open. |
| M3 Verification / evidence | **PRIVATE CI: PERSISTENT-STATE PASS** | Evidence and verifier are separate, VERIFIED is evidence-gated, and the persistence contract has passed on a real ephemeral PostgreSQL 16 engine. Production deployment remains open. |
| M4 Failure / recovery | **PRIVATE CI: INTERRUPTION-RECOVERY PASS** | Current bounded-slice tests cover failure, partial/conflicting evidence, verifier conflict and restart recovery without silent worker replay. Broader external/deployment failure cases remain open. |
| M5 Restart continuity | **PRIVATE CI: PERSISTENT-RESTART PASS** | Persisted state reconstructs the next safe action after interruption and preserves VERIFIED state. Deployed multi-process/provider continuity remains open. |
| M6 Sanitized public proof | **IN PROGRESS** | Public evidence contract, scorecard, review template and pilot protocol exist; final proof package still requires remaining release gates and disclosure review. |

## Latest private CI evidence

A private KORA validation run on 2026-08-28 completed successfully with:

- pinned toolchain/registry verification;
- dependency-lock SHA-256 verification;
- reproducible install with lifecycle scripts disabled;
- high-severity dependency audit;
- real ephemeral PostgreSQL 16 persistence integration;
- 113 automated tests;
- TypeScript typecheck;
- production build.

The bounded private path covers:

```text
PRECHECKED
  -> RUNNING
  -> read-only bounded worker
  -> EVIDENCE_CAPTURED
  -> verifier PASS
  -> VERIFIED
  -> COMPLETED
```

The persistence/recovery tests additionally cover interruption before and after evidence/verifier persistence, exact replay, conflicting replay and preservation of verified canonical state without automatic worker re-execution.

## What the PostgreSQL proof adds

The storage model has now been exercised on a real PostgreSQL engine rather than only through mock adapter responses or textual SQL assertions.

The ephemeral CI database applies the actual migration and tests creation, replay/binding conflicts, evidence/verifier gates, conflicting persistence, completion/failure readback and denial of ambient `PUBLIC` privileges. The database is destroyed after the CI probe.

This does **not** mean a production database is connected.

## What this does NOT prove yet

The result above is a verified private engineering slice, not a released production Golden Demo. It does not yet prove:

- a live application PostgreSQL protocol driver and deployment role;
- arbitrary or consequential external executor authority;
- production-like multi-process/provider continuity;
- independent third-party technical review;
- public third-party reproduction;
- real pilot reliability.

Therefore the correct overall state remains:

```text
GOLDEN DEMO v0.1: NOT YET VERIFIED FOR RELEASE
PUBLICLY REPRODUCED: NO
```

## Release rule

The scorecard may only promote a final release gate when returned evidence supports it. A commit, dispatch, successful-looking output or isolated test is not sufficient by itself.

A public **KORA Golden Demo v0.1** release requires all final M1–M6 acceptance gates to pass, followed by disclosure review of the sanitized package.

---

See also: [KORA Engineering Evidence](KORA_ENGINEERING_EVIDENCE.md), [KORA Evidence Index](KORA_EVIDENCE_INDEX.md) and [KORA Public Showcase](KORA_PUBLIC_SHOWCASE.md).
