# KORA — Public Golden Demo Architecture

**Target:** the smallest public, reproducible KORA runtime that demonstrates the core control loop without exposing the private runtime or security-sensitive implementation.

**Status:** architecture / acceptance contract — implementation pending.

## Goal

A reviewer should be able to clone the public repository and run one bounded command that demonstrates:

```text
GOAL
  -> PLAN
  -> AUTHORITY DECISION
  -> BOUNDED EXECUTION
  -> EVIDENCE CAPTURE
  -> VERIFICATION
  -> PERSISTENT STATE UPDATE
  -> NEXT SAFE ACTION
```

The demo is not intended to prove production readiness. It exists to make a narrow KORA claim publicly reproducible.

## Demo task

Use a deterministic, low-risk, local-only task with no credentials and no external write authority.

Recommended first task:

> Inspect a small local fixture directory, calculate a manifest of allowed files, verify that all paths remain inside the allowed scope, produce a signed/hashed evidence record, persist the terminal state, and report the next valid action.

This task is intentionally ordinary. The value of the demo is the governed lifecycle around the task, not the task itself.

## Required components

### 1. Goal contract

A machine-readable request containing:

- `task_id`;
- human-readable goal;
- allowlisted scope;
- risk level;
- execution budget;
- expected evidence IDs;
- verifier policy.

### 2. Planner

Produces a deterministic plan for this fixed demo class. The public v0.1 demo does not need model inference. Removing model variance makes the control semantics easier to inspect.

### 3. Authority gate

The gate must reject execution when:

- the requested path leaves the fixture scope;
- the task requests mutation rather than the allowed read-only action;
- the budget is missing or invalid;
- required evidence expectations are absent.

### 4. Bounded worker

The worker may only:

- read files inside the demo fixture;
- compute deterministic metadata/hashes;
- write evidence/state only to the demo output directory.

It must not require network access, credentials, shell interpolation from untrusted input, or arbitrary command execution.

### 5. Evidence capture

The successful path should emit at least:

- normalized goal/contract hash;
- plan hash;
- authorization decision;
- inspected-file manifest hash;
- changed-files evidence showing no mutation outside the output area;
- execution result hash;
- verifier binding hash.

### 6. Independent verifier module

The verifier must not trust the worker's `success` field. It independently validates:

- terminal state;
- expected evidence set;
- SHA-256 format and binding;
- provenance values;
- scope/budget decision;
- read-only invariant;
- evidence-set hash;
- expected final state.

### 7. Persistent state

The demo must persist enough state to distinguish:

```text
PREPARED
AUTHORIZED
RUNNING
EVIDENCE_CAPTURED
VERIFIED
COMPLETED
FAILED
BLOCKED
```

A restart/re-run must read the previous terminal state rather than silently pretending the prior run did not exist.

## Required scenarios

The public demo is not complete with only a happy path.

### PASS-01 — valid bounded execution

Expected: `COMPLETED`, verifier `PASS`.

### FAIL-01 — scope escape attempt

Example: request `../outside.txt`.

Expected: blocked before worker execution.

### FAIL-02 — mutation request

Expected: blocked by authority policy.

### FAIL-03 — missing evidence

Expected: verifier `FAIL`; no promotion to `COMPLETED`.

### FAIL-04 — forged verifier/evidence binding

Expected: verifier `FAIL`.

### RECOVERY-01 — interrupted state

Start from a fixture representing an interrupted run after evidence capture but before terminal state promotion.

Expected: reconstruction identifies the next safe action without automatically replaying the worker.

## One-command reviewer experience

Target interface:

```bash
npm ci
npm run golden
```

or, if the demo remains dependency-free:

```bash
npm run golden
```

The command should execute all positive, negative and recovery scenarios and end with a compact result such as:

```text
KORA_PUBLIC_GOLDEN_DEMO=PASS
PASS-01=PASS
FAIL-01=EXPECTED_BLOCK
FAIL-02=EXPECTED_BLOCK
FAIL-03=EXPECTED_VERIFY_FAIL
FAIL-04=EXPECTED_VERIFY_FAIL
RECOVERY-01=PASS
```

## CI requirements

The public workflow should:

- use read-only repository permissions;
- pin third-party actions by commit SHA;
- use a bounded timeout;
- run on pull requests and pushes to `main`;
- avoid secrets;
- avoid network-dependent test behavior after dependency setup;
- fail when an expected negative case unexpectedly succeeds.

## Explicit non-goals for v0.1

The first public runtime should **not** attempt to demonstrate:

- arbitrary external tool execution;
- Gmail/calendar/GitHub mutation;
- production credentials;
- multi-provider model routing;
- autonomous consequential actions;
- production database deployment;
- the full private KORA architecture.

Those would increase attack surface and blur the claim being tested.

## Release acceptance gate

A public Golden Demo v0.1 release is eligible only when all are true:

- [ ] reviewer can run the demo from a clean clone;
- [ ] positive path passes;
- [ ] all negative cases fail for the intended reason;
- [ ] recovery case reconstructs without silent worker replay;
- [ ] CI is green on the release commit;
- [ ] README states the exact public/private claim boundary;
- [ ] no credentials or machine-specific paths are present;
- [ ] an independent reviewer has returned at least one reproducibility result, or the release is explicitly labeled `independent review pending`.

## Why this is the next credibility step

The current public harness reproduces selected evidence semantics. This demo should add one thing that is still missing: a small inspectable runtime in which planning, authority, execution, evidence, verification and continuation are connected end to end.

The ambition remains narrow on purpose: **one claim, one runtime slice, reproducible evidence.**
