# KORA — Engineering Evidence

> **Snapshot date:** 2026-08-28  
> **Purpose:** expose a conservative, non-sensitive engineering evidence surface without publishing private KORA implementation.

This page separates **what is currently evidenced in private engineering** from **what a public reviewer can independently reproduce today**.

## Current evidence level

**Golden Demo status:** M1 contract / precheck evidence exists; end-to-end execution is **not yet publicly verified**.

The current private KORA OPS Golden-task contract is deliberately fail-closed. Its reviewed test contract reaches `PRECHECKED`, returns a PASS precheck verdict for a valid bounded task, and explicitly keeps execution disabled at this milestone.

That distinction matters:

```text
PREPARED ≠ EXECUTED ≠ VERIFIED
```

KORA should not claim a Golden Demo as complete until execution, evidence generation, verification, recovery and restart continuity have all crossed their acceptance gates.

## Private CI evidence — sanitized summary

The private KORA OPS repository currently has an automated validation workflow that performs:

- pinned Node/npm toolchain verification;
- dependency-lock SHA-256 verification;
- reproducible `npm ci` with lifecycle scripts disabled;
- dependency audit at high severity threshold;
- automated tests;
- TypeScript typecheck;
- production build.

A reviewed Golden E2E contract pull request has passed that validation workflow. This remains **private CI evidence**, not a publicly reproducible result.

## Security / failure properties already under automated test

Private tests currently exercise properties including:

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

These are meaningful engineering properties, but they do **not** by themselves prove the entire KORA product loop.

## Golden Demo acceptance ladder

### M1 — Contract + precheck

**State:** evidenced privately.

Required behavior:

- bounded task schema;
- authenticated request;
- exact project/task/state binding;
- security gates pass;
- no arbitrary shell;
- execution remains disabled until the next explicit milestone.

### M2 — Bounded execution

**State:** not yet claimed complete here.

Must demonstrate:

- one allowlisted executor;
- one non-sensitive demo task;
- strict changed-file/runtime/attempt budget;
- no scope expansion;
- immutable starting-state reference;
- explicit execution result.

### M3 — Verification + evidence

**State:** not yet claimed complete here.

Must demonstrate:

- execution result is not trusted automatically;
- artifact hash / changed-files evidence;
- verifier PASS/FAIL result;
- state changes to VERIFIED only after evidence passes;
- failed or insufficient evidence cannot become DONE.

### M4 — Failure injection + recovery

**State:** not yet claimed complete here.

Deliberately inject at least:

- executor failure;
- malformed/insufficient evidence;
- unauthorized scope expansion;
- changed starting state or stale task;
- interrupted run.

Expected outcome:

- fail closed;
- preserve canonical state;
- mark blocked/failed explicitly;
- produce a bounded recovery path;
- never convert failure into a success claim.

### M5 — Restart continuity

**State:** not yet claimed complete here.

Must prove that after process/session restart KORA can reconstruct:

- last verified state;
- unresolved blocker;
- evidence reference;
- next valid action;
- whether new human authority is required.

### M6 — Sanitized public proof

**State:** target.

Publish a deliberately non-sensitive evidence package containing:

- demo version and commit identifier;
- acceptance criteria;
- test categories and result counts;
- CI conclusion;
- failure-injection results;
- hashes of public demo artifacts where safe;
- known limitations;
- exact distinction between private evidence and public reproduction.

No credentials, machine identifiers, private logs, exact security boundaries or proprietary implementation details should be included.

## Release gate

A public **KORA Golden Demo v0.1** should only be announced when all of the following are true:

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
2. issues published with findings and severity;
3. fixes linked to the review findings;
4. a real but low-risk pilot workflow;
5. manual-vs-KORA baseline metrics;
6. repeated runs showing reliability rather than a one-off success.

Useful pilot metrics include user interactions, manual transfers/copy-paste, elapsed time, incorrect state transitions, recovery success and percentage of consequential actions backed by verification evidence.

---

**KORA should earn stronger claims one gate at a time. Evidence before status.**
