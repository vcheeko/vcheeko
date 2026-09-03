# KORA — Engineering Evidence

> **Snapshot date:** 2026-09-03
> **Purpose:** expose a conservative, non-sensitive engineering evidence surface without publishing private KORA implementation.

This page separates **what is currently evidenced in private engineering** from **what a public reviewer can independently reproduce today**.

## Current evidence level

**Golden Demo status:** a bounded private CI vertical slice covers persistent state, verification, interruption recovery, least-privilege database access, adversarial evidence/replay hardening and a real Node-to-PostgreSQL protocol-driver path in ephemeral CI. A small public harness now reproduces selected published evidence semantics. The private runtime is **not** publicly reproduced and the full Golden Demo is **not yet verified for release**.

The central rule remains:

```text
PREPARED ≠ EXECUTED ≠ VERIFIED ≠ PUBLICLY REPRODUCED
```

## Latest private CI evidence — sanitized summary

A private KORA OPS validation candidate on 2026-08-28 completed successfully with:

- pinned Node/npm toolchain and registry verification;
- dependency-lock SHA-256 verification;
- reproducible install with lifecycle scripts disabled;
- dependency audit at high severity threshold with no reported vulnerabilities in that run;
- a real PostgreSQL 16 persistence integration probe against an ephemeral CI database;
- least-privilege database permission/attack checks;
- exact first-Golden evidence/provenance/verifier database hardening;
- terminal replay re-validation of persisted verifier/evidence binding;
- real Node PostgreSQL protocol-driver integration through the bounded database capability;
- non-allowlisted SQL rejection at the driver seam;
- `verify-full` policy for non-loopback database targets;
- isolated transient driver installation followed by restoration of the locked dependency graph;
- transactional application of security-sensitive migration files in CI;
- 119 repository tests discovered: 118 PASS, 0 FAIL, 1 expected skip because the live-driver case is executed separately with a real database in the integration step;
- separate live-driver integration: 2 PASS, 0 FAIL;
- TypeScript typecheck;
- production build.

This remains **private CI evidence**, not public reproduction of the private runtime. Internal credentials, machine identifiers, private logs, exact role names and implementation-sensitive configuration are intentionally not published here.

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
- terminal replay re-validates the exact expected evidence set and persisted verifier binding;
- interruption while `RUNNING` never causes automatic worker replay when evidence is incomplete;
- restart after complete evidence persistence can continue without rerunning the worker;
- restart from `EVIDENCE_CAPTURED` continues through verification using persisted evidence;
- restart after verifier persistence can finalize verification without rewriting the verifier result;
- restart from `VERIFIED` can finalize completion without rewriting verified evidence;
- partial, forged or contradictory persistence state fails closed;
- `COMPLETED` remains unreachable without verified evidence;
- a late failure cannot erase an already verified canonical state.

## Real PostgreSQL + least-privilege evidence

The persistence contract is no longer tested only through mocks or textual SQL checks.

Private CI starts an ephemeral PostgreSQL 16 instance, applies the actual persistence/security definitions and checks:

- new-run creation and exact replay;
- immutable binding conflicts;
- exact evidence and verifier state gates;
- arbitrary evidence-ID rejection;
- forged provenance rejection;
- forged read-only change-set rejection;
- forged verifier-identity rejection;
- exact replay versus conflicting evidence/verifier bindings;
- VERIFIED-state immutability;
- final completion/readback;
- explicit failure persistence;
- absence of ambient public database authority;
- a dedicated runtime capability can use only the intended database interface;
- direct persistence-table reads/writes are denied;
- direct calls to internal persistence routines are denied;
- schema-write authority is denied;
- security-sensitive migration files are exercised transactionally.

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
- non-loopback connections use `verify-full` policy;
- any plaintext exception is restricted to explicit loopback CI use;
- the driver candidate is version/integrity checked before the ephemeral test;
- the transient driver probe is isolated, then the locked repository dependency graph is restored before the main suite.

This demonstrates a **real protocol-driver seam in CI**, not production dependency activation or deployment.

## Productization progress since 2026-08-28

The earlier persistence/database evidence remains part of the engineering baseline. Since that snapshot, private productization work has also produced verified or separately bounded milestones around:

- human-facing Korako journey/workflow UI and a read-only shared-state projection;
- a standalone persistent runner with bounded capabilities and restart handling;
- one-click local launch with startup installation kept behind explicit approval;
- Windows file-lock recovery for Korako-owned shared state;
- hash-bound project writes with stale-precondition rejection and no ambient shell/merge/deploy authority;
- client enforcement of the locked IZBOR/recommendation/no-dead-end presentation rule;
- preparation of a fail-closed LinkedIn read-only OAuth canary while live account connection remains a Human Gate.

The selected Personal Alpha integrated candidate currently has exact-head repository verification of **152/152 tests PASS**, TypeScript typecheck PASS, production build PASS and GitHub validation SUCCESS. It remains **DRAFT / UNMERGED / NON-ACTIVATING** and has not yet received the required physical Windows/PWA Golden acceptance on that exact head.

This distinction is intentional:

```text
CI_PASS != WINDOWS_GOLDEN_VERIFIED
UI_CLICK != EXECUTION_AUTHORITY
```

## Public evidence-contract reproduction

Dependency-free public harnesses are available in this profile repository, including the hardened v0.2 synthetic evidence vectors. A dedicated public product repository now also exists at https://github.com/vcheeko/korako-yolawani; its initial flagship/evidence surface is under draft review and its public evidence workflow has passed on the reviewed draft head.

The public GitHub Actions workflow has executed the harness from the public repository and returned the expected results for synthetic vectors:

- valid terminal evidence/verifier binding → PASS;
- forged verifier evidence-set hash → FAIL;
- missing expected evidence → FAIL.

This makes those **published example semantics** inspectable and reproducible without an npm dependency or private source access.

The hardened v0.2 public suite also rejects forged provenance, non-empty changed-file evidence, wrong verifier identity, duplicate evidence IDs, malformed hashes, contradictory terminal failure state, a non-completed terminal claim and an explicit verifier FAIL verdict. These remain synthetic public evidence-contract checks, not public reproduction of the private KORA runtime.

It does **not** prove that the private KORA runtime is the same implementation, that private CI evidence is genuine, or that production KORA has been reproduced by an independent third party.

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
- arbitrary-shell denial even for an otherwise authoritative source;
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

### M2 — Bounded execution

**Current evidence:** private CI bounded-slice PASS; **production/external executor integration remains open**.

### M3 — Verification + evidence

**Current evidence:** private CI hardened persistent-state + least-privilege DB PASS; **production deployment remains open**.

### M4 — Failure injection + recovery

**Current evidence:** private CI interruption/recovery PASS for the current bounded slice; **broader deployment/external-adapter matrix remains open**.

### M5 — Restart continuity

**Current evidence:** private CI persistent restart/recovery + real protocol-driver PASS for the current bounded slice; **deployed multi-process/provider continuity remains open**.

### M6 — Sanitized public proof

**Current evidence:** public evidence-contract harness CI PASS; **independent returned evidence and private-runtime reproduction remain open**.

The public harness is a stronger evidence surface than prose alone, but it must not be upgraded into a claim that private KORA has been independently reproduced.

## What is explicitly NOT claimed

The current evidence does not yet prove:

- a production-ready autonomous system;
- arbitrary or consequential executor authority;
- a production database, production credential or deployed database principal;
- a committed/activated production PostgreSQL driver dependency;
- production-like multi-process/provider continuity;
- independent third-party technical/security acceptance;
- public reproduction of the private runtime;
- real pilot reliability or product-market fit.

## Release gate

A public **KORA Golden Demo v0.1** should only be announced when remaining review, disclosure and pilot/release gates support that exact claim.

Until then, the correct public status is **prototype / evidence-building**.

## What comes next

The current credibility ladder is:

1. physical exact-head Windows/PWA Golden acceptance for the selected Personal Alpha candidate;
2. a minimal clean-clone public Golden runtime with positive, negative and recovery scenarios;
3. independent technical/security review with findings recorded by severity;
4. fixes linked to returned findings;
5. independent reproduction against the public runtime and reviewed private scope where appropriate;
6. a real but low-risk pilot workflow;
7. manual-vs-Korako baseline metrics and repeated reliability runs.

Useful pilot metrics include user interactions, manual inter-system relay/copy-paste, elapsed time, incorrect state transitions, duplicate execution, recovery success and percentage of terminal claims backed by expected verification evidence. Meaningful Human Gates remain separate from avoidable relay friction, and no time-saved claim should be emitted without a verified comparable baseline.

---

**KORA should earn stronger claims one gate at a time. Evidence before status.**
