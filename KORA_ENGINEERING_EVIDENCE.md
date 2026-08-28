# KORA — Engineering Evidence

> **Snapshot date:** 2026-08-28  
> **Purpose:** expose a conservative, non-sensitive engineering evidence surface without publishing private KORA implementation.

This page separates **what is currently evidenced in private engineering** from **what a public reviewer can independently reproduce today**.

## Current evidence level

**Golden Demo status:** a bounded private CI vertical slice now exists, but the full Golden Demo runtime is **not yet verified for release and is not publicly reproducible**.

The central rule remains:

```text
PREPARED ≠ EXECUTED ≠ VERIFIED ≠ PUBLICLY REPRODUCED
```

## Latest private CI evidence — sanitized summary

Private KORA OPS commit `bd73fbf092005b4f48115f3aa39260a9bb937b12` passed the `validate` GitHub Actions workflow on 2026-08-28.

That validation pipeline performs:

- pinned Node/npm toolchain and registry verification;
- dependency-lock SHA-256 verification;
- reproducible `npm ci` with lifecycle scripts disabled;
- dependency audit at high severity threshold;
- automated tests;
- TypeScript typecheck;
- production build.

All of those workflow steps completed successfully for the cited vertical-slice commit. This is **private CI evidence**, not a public-source reproduction.

## What the current private vertical slice exercises

The Golden E2E test path now models:

```text
PRECHECKED
  → RUNNING
  → bounded read-only worker
  → EVIDENCE_CAPTURED
  → independent verifier state
  → VERIFIED
  → COMPLETED
```

The first worker capability is deliberately narrow: it audits only the expected files supplied through an explicitly bounded server-side snapshot. It does not spawn processes, execute shell commands, perform network I/O or mutate project state.

Current private tests additionally check that:

- a persisted `RUNNING` state cannot execute twice;
- missing required worker input fails closed;
- insufficient evidence cannot become VERIFIED;
- COMPLETED is unreachable before verifier PASS;
- run state can be serialized/restored with integrity protection;
- state tampering is rejected;
- an injected worker failure remains FAILED after restore;
- human briefing is projected from canonical run state rather than conversational memory.

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

**Current evidence:** private CI vertical-slice PASS; **final runtime gate remains open**.

The vertical slice demonstrates one deterministic allowlisted read-only worker over a bounded snapshot with no ambient filesystem/network/shell authority. Remaining work includes integration with the intended canonical persistent runtime path rather than only the isolated test slice.

### M3 — Verification + evidence

**Current evidence:** private CI state-gate PASS; **final persistence/integration gate remains open**.

The slice keeps worker evidence separate from the verifier state, requires evidence before VERIFIED, and makes COMPLETED unreachable without verifier PASS. Canonical persistent evidence-store integration remains to be completed.

### M4 — Failure injection + recovery

**Current evidence:** limited private CI PASS; **full matrix remains open**.

Current tests cover missing worker input, insufficient evidence, replay blocking, state tamper rejection and explicit worker failure. Still required: broader interrupted-run, stale-state, malformed evidence, unauthorized expansion and bounded recovery scenarios through the integrated runtime path.

### M5 — Restart continuity

**Current evidence:** private serialization/restore PASS; **canonical persistent-store restart gate remains open**.

The test slice can serialize and restore content-addressed run state and reject tampering. Full process/provider restart reconstruction must still be proven from canonical persistence rather than an in-test serialized envelope alone.

### M6 — Sanitized public proof

**Current evidence:** public evidence surface IN PROGRESS.

This profile now contains the public showcase, evidence index, scorecard, review template, pilot protocol and evidence changelog. A final Golden Demo proof package must wait for the remaining integrated runtime gates.

## What is explicitly NOT claimed

The current evidence does not yet prove:

- a production-ready autonomous system;
- arbitrary or consequential executor authority;
- a complete persistent runtime Golden Demo;
- the entire failure/recovery matrix;
- independent third-party technical acceptance;
- public third-party reproduction;
- product-market fit.

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
