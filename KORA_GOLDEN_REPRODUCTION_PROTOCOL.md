# KORA Golden Demo — Public Reproduction Protocol

**Status:** public protocol / evidence-contract reproduction  
**Current harness:** v0.2 adversarial vectors  
**Private runtime source:** not published  
**Public reproduction of production KORA:** NO

This protocol gives an external reviewer something concrete to run without publishing the private KORA implementation or security-sensitive deployment details.

Its purpose is narrow:

> reproduce selected **public verification semantics**, not claim independent reproduction of the private runtime.

## Core rule

```text
A terminal state label is not sufficient proof.
```

The public verifier accepts a synthetic terminal vector only when all published example-contract requirements remain bound.

## v0.2 verification contract

For a public vector marked `COMPLETED`, v0.2 requires:

1. the exact expected evidence IDs;
2. no duplicate evidence IDs;
3. lowercase SHA-256-shaped content hashes;
4. exact expected provenance for each evidence ID;
5. the published `changed_files` evidence hash representing an empty JSON array (`[]`) for this read-only example;
6. verifier verdict `PASS`;
7. the exact canonical public-example verifier identity;
8. a verifier evidence-set hash recomputed from evidence IDs, content hashes and provenance;
9. no failure code on a successful terminal vector.

The canonical evidence-set representation is:

```text
sort by evidence_id
then join each record as:
<evidence_id>:<content_hash>:<provenance>
with newline separators
then SHA-256 the UTF-8 bytes
```

## Reason-bound negative testing

v0.2 does not count “any failure” as a successful negative test.

Each vector declares both:

- expected outcome (`PASS` or `FAIL`); and
- expected reason code.

The suite fails if a negative vector fails for the wrong reason. This prevents an unrelated bug from accidentally satisfying an adversarial test.

## Included v0.2 vectors

`public-evidence/kora-golden-v0.2/golden-public-vectors.json` contains intentionally synthetic cases for:

- valid terminal evidence — PASS;
- forged verifier evidence-set hash — FAIL;
- missing expected evidence — FAIL;
- forged provenance with an otherwise matching verifier hash — FAIL;
- non-empty `changed_files` evidence with an otherwise matching verifier hash — FAIL;
- wrong verifier identity — FAIL;
- duplicate evidence ID — FAIL;
- malformed content hash — FAIL;
- terminal failure code attached to `COMPLETED` — FAIL;
- `VERIFIED` state presented as if terminal completion — FAIL;
- verifier `FAIL` verdict — FAIL.

The vectors do not come from a private production database and do not disclose private KORA evidence values, role names, credentials or machine identifiers.

## Run it

Requires Node.js with the standard library only; no npm install is needed.

Current v0.2:

```bash
node public-evidence/kora-golden-v0.2/verify.mjs
```

Expected final output:

```text
KORA_PUBLIC_VECTOR_SUITE_V0_2=PASS
```

Historical v0.1 remains runnable for regression/reference:

```bash
node public-evidence/kora-golden-v0.1/verify.mjs
```

The public GitHub Actions workflow runs both versions.

## What a successful v0.2 run proves

It confirms that the **published public example harness** currently enforces its declared semantics for:

- exact evidence-set completeness;
- duplicate-evidence rejection;
- evidence hash shape;
- provenance binding;
- read-only changed-files evidence;
- terminal-state consistency;
- verifier verdict and identity;
- verifier-to-evidence binding;
- exact expected failure classes for adversarial vectors.

It is useful because a reviewer can inspect a small verifier and synthetic vectors directly instead of accepting screenshots or prose claims.

## What this does NOT prove

The harness does not prove:

- that private KORA source is identical to the public verifier;
- that a private CI result occurred or is genuine;
- that private evidence is genuine;
- production deployment;
- production database ownership or credentials;
- runtime availability or scale;
- arbitrary/consequential executor safety;
- independent technical/security acceptance;
- independent reproduction of the private runtime;
- pilot reliability;
- product-market fit.

Therefore the correct status remains:

```text
PUBLIC EVIDENCE-CONTRACT HARNESS: REPRODUCIBLE
PRIVATE RUNTIME PUBLICLY REPRODUCED: NO
INDEPENDENT THIRD-PARTY ACCEPTANCE: NO
GOLDEN DEMO RELEASE VERIFIED: NO
```

## External reviewer protocol

An external reviewer can strengthen the evidence by:

1. cloning the public repository at a recorded commit;
2. reviewing `verify.mjs` and the vectors before execution;
3. running both v0.1 and v0.2 in a clean environment;
4. recording Node version, commit SHA, output and timestamp;
5. creating additional negative vectors without changing expected outcomes to force PASS;
6. reporting discrepancies and exact failure reasons;
7. documenting what was not tested.

A useful independent result should include failures found as well as successes. Generic praise is not evidence.

## M6 progression

M6 remains open until an identifiable independent reviewer returns evidence and the disclosure boundary is reviewed. Public CI is useful evidence, but it is not an independent third-party review.

A future stronger package may add signed attestations or additional sanitized failure vectors, while private credentials, machine identifiers, internal role names and implementation-sensitive boundaries remain private.
