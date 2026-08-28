# KORA — Engineering Evidence

> **Snapshot date:** 2026-08-28  
> **Purpose:** expose a conservative, non-sensitive engineering evidence surface without publishing private KORA implementation.

This page separates **what is currently evidenced in private engineering** from **what a public reviewer can independently reproduce today**.

## Current evidence level

**Golden Demo status:** a bounded private CI vertical slice now covers persistent state, verification, interruption recovery, least-privilege database access and a real Node-to-PostgreSQL protocol-driver path in ephemeral CI. The full Golden Demo is **not yet verified for release and is not publicly reproducible**.

The central rule remains:

```text
PREPARED ≠ EXECUTED ≠ VERIFIED ≠ PUBLICLY REPRODUCED
```

## Latest private CI evidence — sanitized summary

A private KORA OPS validation run on 2026-08-28 completed successfully with:

- pinned Node/npm toolchain and registry verification;
- dependency-lock SHA-256 verification;
- reproducible install with lifecycle scripts disabled;
- dependency audit at high severity threshold;
- a real PostgreSQL 16 persistence integration probe against an ephemeral CI database;
- least-privilege database permission/attack checks;
- a real Node PostgreSQL protocol-driver integration through that bounded database capability;
- 115 repository tests discovered: 114 PASS, 0 FAIL, 1 expected skip because the live-driver case is executed separately with a real database in the integration step;
- TypeScript typecheck;
- production build.

This remains **private CI evidence**, not a public-source reproduction. Internal credentials, machine identifiers, private logs, exact role names and implementation-sensitive configuration are intentionally not published here.

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

The persistence/recovery path exercises properties including:

- exact task and starting-state binding;
- replay-safe completion: an exact completed request returns its prior result rather than running the worker again;
- interruption while `RUNNING` never causes automatic worker replay when evidence is incomplete;
- restart after complete evidence persistence can continue without rerunning the worker;
- restart from `EVIDENCE_CAPTURED` continues through verification using persisted evidence;
- restart after verifier persistence can finalize verification without rewriting the verifier result;
- restart from `VERIFIED` can finalize completion without rewriting verified evidence;
- partial or contradictory persistence state fails closed;
- `COMPLETED` remains unreachable without verified evidence;
- a late failure cannot erase an already verified canonical state.

## Real PostgreSQL + least-privilege evidence

The persistence contract is no longer tested only through mocks or textual SQL checks.

Private CI starts an ephemeral PostgreSQL 16 instance, applies the actual persistence and permission definitions, then checks:

- new-run creation and exact replay;
- immutable binding conflicts;
- evidence and verifier state gates;
- exact replay versus conflicting evidence/verifier bindings;
- VERIFIED-state immutability;
- final completion/readback;
- explicit failure persistence;
- absence of ambient public database authority;
- a dedicated runtime capability can use only the intended database interface;
- direct persistence-table reads/writes are denied to that runtime capability;
- direct calls to internal persistence routines are denied;
- schema-write authority is denied.

The database, test principal and runtime capability are temporary and removed after the proof. No production database or production credential is activated by this evidence.

## Real Node protocol-driver evidence

The bounded runtime store has also been exercised over an actual Node PostgreSQL protocol connection in ephemeral CI.

The private proof checks that:

- the application-side store reaches persistence only through the least-privilege database interface;
- database calls are schema-qualified;
- the driver accepts only a small exact SQL allowlist owned by the application code;
- query values remain separate from SQL text;
- non-allowlisted SQL is rejected before database execution;
- exact replay returns the previously persisted result without duplicate worker execution;
- non-local connections are TLS-required by default;
- any plaintext exception is restricted to explicit loopback CI use;
- the driver candidate is version/integrity checked before the ephemeral test;
- the transient probe does not modify the committed dependency graph.

This demonstrates a **real protocol-driver seam in CI**, not production dependency activation or deployment.

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

**Current evidence:** private CI bounded-slice PASS; **production/external executor integration remains open**.

One deterministic allowlisted read-only worker executes within an explicit bounded contract, and replay/restart logic prevents accidental duplicate execution. The public claim does not extend to arbitrary or consequential executor authority.

### M3 — Verification + evidence

**Current evidence:** private CI persistent-state + least-privilege DB PASS; **production deployment remains open**.

Worker evidence is stored separately from verifier state; VERIFIED requires expected evidence and verifier PASS; COMPLETED remains gated behind VERIFIED. Persistence and permission boundaries have been exercised against a real ephemeral PostgreSQL engine.

### M4 — Failure injection + recovery

**Current evidence:** private CI interruption/recovery PASS for the current bounded slice; **broader deployment/external-adapter matrix remains open**.

The current matrix covers worker failure, partial evidence, evidence conflict, verifier conflict, replay, state tampering, restart after evidence persistence, restart after verifier persistence and preservation of VERIFIED state.

### M5 — Restart continuity

**Current evidence:** private CI persistent restart/recovery + real protocol-driver PASS for the current bounded slice; **deployed multi-process/provider continuity remains open**.

Persisted state reconstructs the next safe action without relying on conversational memory or silently rerunning the worker, and the bounded path has been exercised through a real Node-to-PostgreSQL connection in ephemeral CI.

### M6 — Sanitized public proof

**Current evidence:** public evidence surface IN PROGRESS.

This profile contains the public showcase, evidence index, scorecard, review template, pilot protocol and evidence changelog. Final Golden Demo publication still requires the remaining release gates and disclosure review.

## What is explicitly NOT claimed

The current evidence does not yet prove:

- a production-ready autonomous system;
- arbitrary or consequential executor authority;
- a production database, production credential or deployed database principal;
- a committed/activated production PostgreSQL driver dependency;
- production-like multi-process/provider continuity;
- independent third-party technical/security acceptance;
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

1. independent technical/security review;
2. findings recorded by severity rather than hidden;
3. fixes linked to those findings;
4. a real but low-risk pilot workflow;
5. manual-vs-KORA baseline metrics;
6. repeated runs showing reliability rather than a one-off success.

Useful pilot metrics include user interactions, manual transfers/copy-paste, elapsed time, incorrect state transitions, recovery success and percentage of consequential actions backed by verification evidence.

---

**KORA should earn stronger claims one gate at a time. Evidence before status.**
