# KORA — Engineering Evidence

> **Snapshot date:** 2026-08-28  
> **Purpose:** expose a conservative, non-sensitive engineering evidence surface without publishing private KORA implementation.

This page separates **what is currently evidenced in private engineering** from **what a public reviewer can independently reproduce today**.

## Current evidence level

**Golden Demo status:** a bounded private CI vertical slice now covers persistent state, verification, interruption recovery and a real ephemeral PostgreSQL engine. The full Golden Demo is **not yet verified for release and is not publicly reproducible**.

The central rule remains:

```text
PREPARED ≠ EXECUTED ≠ VERIFIED ≠ PUBLICLY REPRODUCED
```

## Latest private CI evidence — sanitized summary

A private KORA OPS validation run on 2026-08-28 completed successfully with:

- pinned Node/npm toolchain and registry verification;
- dependency-lock SHA-256 verification;
- reproducible `npm ci` with lifecycle scripts disabled;
- dependency audit at high severity threshold;
- a real PostgreSQL 16 persistence integration probe against an ephemeral CI database;
- 113 automated tests;
- TypeScript typecheck;
- production build.

This remains **private CI evidence**, not a public-source reproduction. Internal credentials, machine identifiers, private logs and implementation-sensitive configuration are intentionally not published here.

## What the current private vertical slice exercises

The Golden path models:

```text
PRECHECKED
  → RUNNING
  → bounded read-only worker
  → EVIDENCE_CAPTURED
  → verifier PASS
  → VERIFIED
  → COMPLETED
```

The first worker capability is deliberately narrow: it operates only on explicitly bounded input, has no arbitrary shell authority, performs no ambient network activity and cannot silently expand scope.

The persistence/recovery path now exercises properties including:

- exact task and starting-state binding;
- replay-safe completion: an exact completed request returns its prior result rather than running the worker again;
- interruption while `RUNNING` never causes automatic worker replay when evidence is incomplete;
- a restart after complete evidence persistence can continue from that evidence without rerunning the worker;
- a restart from `EVIDENCE_CAPTURED` continues through verification using persisted evidence;
- a restart after verifier persistence can finalize verification without rerunning the verifier write;
- a restart from `VERIFIED` can finalize completion without rewriting verified evidence;
- partial or contradictory persistence state fails closed;
- `COMPLETED` remains unreachable without verified evidence;
- a late failure cannot erase an already verified canonical state.

## Real PostgreSQL engine evidence

The persistence contract is no longer tested only through mocks or textual SQL checks.

Private CI now starts an ephemeral PostgreSQL 16 instance available on the CI runner, applies the actual migration and exercises the real database functions for:

- new-run creation and exact replay;
- immutable binding conflicts;
- evidence-state gates;
- exact evidence replay versus conflicting evidence;
- verifier-state gates;
- exact verifier replay versus conflicting verifier binding;
- VERIFIED-state immutability;
- final completion/readback;
- explicit failure persistence;
- denial of ambient `PUBLIC` privileges for Golden persistence tables and routines.

The database is temporary and removed after the test. No production database, application credentials or deployment role are activated by this proof.

## Broader security properties already under automated test

Private tests also exercise properties including:

- exact bounded Golden-task schema;
- rejection of arbitrary command/shell fields;
- path-escape rejection;
- no ambient authority;
- fail-closed behavior when server configuration is missing;
- HMAC request authentication;
- requester binding;
- project, task-package and starting-state binding;
- mutation detection after task-hash binding;
- autonomy-budget enforcement;
- arbitrary shell denial;
- destructive-action denial;
- encoded/path-traversal scope rejection;
- web/file prompt-injection treated as untrusted input;
- agent output not becoming ambient write authority;
- signed local-agent evidence not automatically gaining write/dispatch authority;
- narrow allowlisted egress;
- secret/sensitive-data egress blocking;
- dependency-change requirements for digest pinning, provenance and disabled install scripts;
- tool/depth/retry/wall-clock autonomy limits.

## Golden Demo acceptance ladder

### M1 — Contract + precheck

**Current evidence:** private PASS.

The bounded authenticated task contract, project/task/state binding and fail-closed precheck are covered by private tests.

### M2 — Bounded execution

**Current evidence:** private CI vertical-slice PASS; **production/application integration remains open**.

One deterministic allowlisted read-only worker executes within an explicit bounded contract, and replay/restart logic prevents accidental duplicate execution. The public claim does not extend to arbitrary or consequential executor authority.

### M3 — Verification + evidence

**Current evidence:** private CI persistent-state PASS; **production deployment remains open**.

Worker evidence is stored separately from verifier state; VERIFIED requires the expected evidence and verifier PASS; COMPLETED remains gated behind VERIFIED. The persistence model has also been exercised against a real ephemeral PostgreSQL engine.

### M4 — Failure injection + recovery

**Current evidence:** private CI interruption/recovery PASS for the current bounded slice; **broader production matrix remains open**.

The current matrix covers worker failure, partial evidence, evidence conflict, verifier conflict, replay, state tampering, restart after evidence persistence, restart after verifier persistence and preservation of VERIFIED state. Broader external-adapter and deployment failures are still future gates.

### M5 — Restart continuity

**Current evidence:** private CI persistent restart/recovery PASS for the current bounded slice; **deployed multi-process/provider continuity remains open**.

Canonical persisted state can reconstruct whether the next safe action is recovery, verification, finalization or manual review without relying on conversational memory and without silently rerunning the worker.

### M6 — Sanitized public proof

**Current evidence:** public evidence surface IN PROGRESS.

This profile contains the public showcase, evidence index, scorecard, review template, pilot protocol and evidence changelog. The final Golden Demo proof package still requires the remaining release gates and disclosure review.

## What is explicitly NOT claimed

The current evidence does not yet prove:

- a production-ready autonomous system;
- arbitrary or consequential executor authority;
- a live production database or application DB role;
- a live application PostgreSQL driver exercising the runtime adapter in deployment;
- independent third-party technical acceptance;
- public third-party reproduction;
- real pilot reliability or product-market fit.

## Release gate

A public **KORA Golden Demo v0.1** should only be announced when the final acceptance matrix is:

```text
M1 CONTRACT/PRECHECK ........ PASS
M2 BOUNDED EXECUTION ........ PASS
M3 VERIFICATION/EVIDENCE .... PASS
M4 FAILURE/RECOVERY ......... PASS
M5 RESTART CONTINUITY ....... PASS
M6 PUBLIC PROOF PACKAGE ..... PASS
```

Until then, the correct public status is **prototype / evidence-building**.

## What comes after the Golden Demo

The credibility ladder after v0.1 is:

1. independent technical review;
2. findings recorded by severity rather than hidden;
3. fixes linked to those findings;
4. a real but low-risk pilot workflow;
5. manual-vs-KORA baseline metrics;
6. repeated runs showing reliability rather than a one-off success.

Useful pilot metrics include user interactions, manual transfers/copy-paste, elapsed time, incorrect state transitions, recovery success and percentage of consequential actions backed by verification evidence.

---

**KORA should earn stronger claims one gate at a time. Evidence before status.**
