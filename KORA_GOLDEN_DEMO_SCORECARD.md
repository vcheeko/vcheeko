# KORA Golden Demo — Public Scorecard

**Status date:** 2026-08-28  
**Public claim level:** prototype / evidence-building

This scorecard is intentionally conservative. It separates a **private CI vertical-slice proof** from a finished runtime or a publicly reproducible Golden Demo.

| Gate | Public status | Meaning |
| --- | --- | --- |
| M1 Contract / precheck | **PRIVATE EVIDENCE: PASS** | Bounded authenticated Golden-task contract and fail-closed precheck exist in private engineering. |
| M2 Bounded execution | **PRIVATE CI: VERTICAL-SLICE PASS** | One deterministic allowlisted read-only worker capability executes over an explicitly bounded server-side snapshot. Real persistent/runtime adapter integration remains open. |
| M3 Verification / evidence | **PRIVATE CI: STATE-GATE PASS** | Evidence capture, independent verifier state and `COMPLETED`-only-after-`VERIFIED` transition are covered in the private vertical slice. Persistent production evidence storage remains open. |
| M4 Failure / recovery | **PRIVATE CI: LIMITED PASS** | Missing worker input, insufficient evidence, replay and tamper/failure paths fail closed in tests. The full injected-failure matrix is not complete. |
| M5 Restart continuity | **PRIVATE CI: SERIALIZATION PASS** | Content-addressed run state survives serialize/restore and rejects tampering in the vertical slice. Canonical persistent-store/process restart integration remains open. |
| M6 Sanitized public proof | **IN PROGRESS** | Public evidence contract, scorecard, review template and pilot protocol exist; final proof package depends on runtime integration and the remaining release gates. |

## Latest private CI evidence

A private Golden E2E branch at commit `bd73fbf092005b4f48115f3aa39260a9bb937b12` passed the repository validation workflow on 2026-08-28.

The successful workflow includes:

- pinned toolchain/registry verification;
- dependency-lock SHA-256 verification;
- reproducible install with lifecycle scripts disabled;
- high-severity dependency audit;
- automated tests;
- TypeScript typecheck;
- production build.

The private E2E test slice covers:

```text
PRECHECKED
  -> RUNNING
  -> read-only bounded worker
  -> EVIDENCE_CAPTURED
  -> verifier PASS
  -> VERIFIED
  -> COMPLETED
```

It also checks that a failed worker path remains failed after state restore and that a persisted `RUNNING` state cannot execute twice.

## What this does NOT prove yet

The result above is a **test/CI vertical slice**, not a finished Golden Demo runtime. It does not yet prove:

- a production-like persistent work-order/evidence store across the complete runtime path;
- a real external worker adapter operating under the same bounded contract;
- the complete M4 injected-failure matrix;
- full process/provider restart reconstruction from the canonical persistent store;
- independent third-party technical review;
- public third-party reproduction.

Therefore the correct overall state remains:

```text
GOLDEN DEMO v0.1: NOT YET VERIFIED FOR RELEASE
PUBLICLY REPRODUCED: NO
```

## Release rule

The scorecard may only promote a release gate when returned evidence supports it. A commit, dispatch, successful-looking output or isolated test is not sufficient by itself.

A public **KORA Golden Demo v0.1** release requires all final M1–M6 acceptance gates to pass, followed by disclosure review of the sanitized package.

---

See also: [KORA Engineering Evidence](KORA_ENGINEERING_EVIDENCE.md), [KORA Evidence Index](KORA_EVIDENCE_INDEX.md) and [KORA Public Showcase](KORA_PUBLIC_SHOWCASE.md).
