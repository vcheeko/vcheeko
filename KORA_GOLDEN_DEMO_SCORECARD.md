# KORA Golden Demo — Public Scorecard

**Status date:** 2026-08-28  
**Public claim level:** prototype / evidence-building

This scorecard is intentionally conservative. It separates a **private CI verified vertical slice** from a finished production runtime or a publicly reproducible Golden Demo.

| Gate | Public status | Meaning |
| --- | --- | --- |
| M1 Contract / precheck | **PRIVATE EVIDENCE: PASS** | Bounded authenticated Golden-task contract and fail-closed precheck exist in private engineering. |
| M2 Bounded execution | **PRIVATE CI: BOUNDED-SLICE PASS** | One deterministic allowlisted read-only worker executes within explicit scope/budget with replay protection. Arbitrary/consequential external executor integration remains open. |
| M3 Verification / evidence | **PRIVATE CI: PERSISTENCE + LEAST-PRIVILEGE PASS** | Evidence/verifier are separate, VERIFIED is evidence-gated, real PostgreSQL persistence works, and direct database authority is denied outside a narrow runtime capability. Production deployment remains open. |
| M4 Failure / recovery | **PRIVATE CI: INTERRUPTION-RECOVERY PASS** | Current bounded-slice tests cover failure, partial/conflicting evidence, verifier conflict and restart recovery without silent worker replay. Broader external/deployment failure cases remain open. |
| M5 Restart continuity | **PRIVATE CI: PERSISTENT-RESTART + PROTOCOL-DRIVER PASS** | Persisted state reconstructs the next safe action and the bounded runtime path has been exercised through a real Node-to-PostgreSQL connection in ephemeral CI. Deployed multi-process/provider continuity remains open. |
| M6 Sanitized public proof | **IN PROGRESS** | Public evidence contract, scorecard, review template and pilot protocol exist; final proof package still requires remaining release gates, independent review and disclosure review. |

## Latest private CI evidence

A private KORA validation run on 2026-08-28 completed successfully with:

- pinned toolchain/registry verification;
- dependency-lock SHA-256 verification;
- reproducible install with lifecycle scripts disabled;
- high-severity dependency audit;
- real ephemeral PostgreSQL 16 persistence integration;
- least-privilege database permission/attack checks;
- real Node PostgreSQL protocol-driver integration through the restricted database interface;
- driver-level rejection of non-allowlisted SQL;
- 115 repository tests discovered: 114 PASS, 0 FAIL, 1 expected skip because the live-driver case is exercised separately against a real database in the integration step;
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

The persistence/recovery tests cover interruption before and after evidence/verifier persistence, exact replay, conflicting replay and preservation of verified canonical state without automatic worker re-execution.

## What the database/driver proof adds

The storage path has now been exercised beyond mock adapter responses or textual SQL assertions:

- the actual persistence definitions run on a real PostgreSQL 16 engine;
- a restricted runtime database capability can use the intended interface while direct table/core/schema-write paths are denied;
- an ephemeral low-privilege application principal uses a real Node PostgreSQL protocol connection;
- exact replay returns the existing result without duplicate worker execution;
- application-side SQL is constrained to a small fixed allowlist;
- non-local driver targets require TLS by default;
- the transient driver candidate is version/integrity checked and does not change the committed dependency graph.

The database and test identities are destroyed after the CI probe. This does **not** mean a production database or production driver dependency is activated.

## What this does NOT prove yet

The result above is a verified private engineering slice, not a released production Golden Demo. It does not yet prove:

- production database credentials or a deployed database principal;
- committed/activated production PostgreSQL driver dependency;
- arbitrary or consequential external executor authority;
- production-like multi-process/provider continuity;
- independent third-party technical/security review;
- public third-party reproduction;
- real pilot reliability.

Therefore the correct overall state remains:

```text
GOLDEN DEMO v0.1: NOT YET VERIFIED FOR RELEASE
PUBLICLY REPRODUCED: NO
```

## Release rule

The scorecard may only promote a final release gate when returned evidence supports it. A commit, dispatch, successful-looking output or isolated test is not sufficient by itself.

A public **KORA Golden Demo v0.1** release requires all final M1–M6 acceptance gates to pass, followed by independent/disclosure review of the sanitized package.

---

See also: [KORA Engineering Evidence](KORA_ENGINEERING_EVIDENCE.md), [KORA Evidence Index](KORA_EVIDENCE_INDEX.md) and [KORA Public Showcase](KORA_PUBLIC_SHOWCASE.md).
