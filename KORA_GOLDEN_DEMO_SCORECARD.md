# KORA Golden Demo — Public Scorecard

**Status date:** 2026-08-28  
**Public claim level:** prototype / evidence-building

This scorecard is intentionally conservative. It separates a **private CI verified vertical slice** from a finished production runtime or a publicly reproduced Golden Demo.

| Gate | Public status | Meaning |
| --- | --- | --- |
| M1 Contract / precheck | **PRIVATE EVIDENCE: PASS** | Bounded authenticated Golden-task contract and fail-closed precheck exist in private engineering. |
| M2 Bounded execution | **PRIVATE CI: BOUNDED-SLICE PASS** | One deterministic allowlisted read-only worker executes within explicit scope/budget with replay protection. Arbitrary/consequential external executor integration remains open. |
| M3 Verification / evidence | **PRIVATE CI: HARDENED PERSISTENCE + LEAST-PRIVILEGE PASS** | Evidence/verifier are separate, terminal replay is revalidated, real PostgreSQL persistence works, and direct database authority is denied outside a narrow runtime capability. Production deployment remains open. |
| M4 Failure / recovery | **PRIVATE CI: INTERRUPTION-RECOVERY PASS** | Current bounded-slice tests cover failure, partial/conflicting evidence, verifier conflict and restart recovery without silent worker replay. Broader external/deployment failure cases remain open. |
| M5 Restart continuity | **PRIVATE CI: PERSISTENT-RESTART + PROTOCOL-DRIVER PASS** | Persisted state reconstructs the next safe action and the bounded runtime path has been exercised through a real Node-to-PostgreSQL connection in ephemeral CI. Deployed multi-process/provider continuity remains open. |
| M6 Sanitized public proof | **PUBLIC HARNESS CI: PASS · INDEPENDENT RETURNED EVIDENCE PENDING** | A dependency-free public evidence-contract harness with positive/negative vectors is on `main` and passes public CI. This is not reproduction of the private runtime; an independent reviewer still needs to return evidence. |

## Latest private CI evidence — sanitized

A private KORA validation candidate on 2026-08-28 completed successfully with:

- pinned toolchain/registry verification;
- dependency-lock SHA-256 verification;
- reproducible install with lifecycle scripts disabled;
- high-severity dependency audit with no reported vulnerabilities at that run;
- real ephemeral PostgreSQL 16 persistence integration;
- least-privilege database permission/attack checks;
- exact first-Golden evidence/provenance/verifier database gates;
- terminal replay re-validation of evidence/verifier binding;
- real Node PostgreSQL protocol-driver integration through the restricted database interface;
- driver-level rejection of non-allowlisted SQL;
- authenticated `verify-full` policy for non-loopback DB targets;
- isolated transient driver probe followed by a fresh locked dependency install;
- transactional application of security-sensitive migration files in CI;
- 119 repository tests discovered: 118 PASS, 0 FAIL, 1 expected skip because the live-driver case is exercised separately against a real database in the integration step;
- separate live-driver integration: 2 PASS, 0 FAIL;
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

The persistence/recovery tests cover interruption before and after evidence/verifier persistence, exact replay, conflicting replay, forged terminal verifier binding and preservation of verified canonical state without automatic worker re-execution.

## Public evidence-contract harness

The public repository now contains:

- [KORA Golden Reproduction Protocol](KORA_GOLDEN_REPRODUCTION_PROTOCOL.md);
- dependency-free Node verifier;
- a valid terminal vector expected to PASS;
- forged-verifier-hash vector expected to FAIL;
- missing-evidence vector expected to FAIL;
- read-only GitHub Actions workflow.

The public CI run on the merged harness returned the expected positive and negative outcomes and completed successfully.

This makes selected **published evidence semantics reproducible from public source**. It does **not** prove that the private runtime is the same implementation, that private evidence is genuine, or that production KORA has been independently reproduced.

## What this does NOT prove yet

The current evidence does not yet prove:

- production database credentials or a deployed database principal;
- committed/activated production PostgreSQL driver dependency;
- dedicated production object ownership/default-privilege configuration;
- arbitrary or consequential external executor authority;
- production-like multi-process/provider continuity;
- independent third-party technical/security acceptance;
- independent returned reproduction evidence for the private runtime;
- real pilot reliability.

Therefore the correct overall state remains:

```text
GOLDEN DEMO v0.1: NOT YET VERIFIED FOR RELEASE
PUBLIC EVIDENCE-CONTRACT HARNESS: VERIFIED ON PUBLIC CI
PRIVATE RUNTIME PUBLICLY REPRODUCED: NO
```

## Release rule

The scorecard may only promote a final release gate when returned evidence supports it. A commit, dispatch, successful-looking output or isolated self-test is not sufficient by itself.

A public **KORA Golden Demo v0.1** release requires the remaining independent-review, disclosure and pilot/release decisions to be satisfied for the claimed scope.

---

See also: [KORA Engineering Evidence](KORA_ENGINEERING_EVIDENCE.md), [KORA Evidence Index](KORA_EVIDENCE_INDEX.md), [KORA Golden Reproduction Protocol](KORA_GOLDEN_REPRODUCTION_PROTOCOL.md) and [KORA Public Showcase](KORA_PUBLIC_SHOWCASE.md).
