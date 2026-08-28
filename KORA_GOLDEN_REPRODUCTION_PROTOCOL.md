# KORA Golden Demo — Public Reproduction Protocol

**Status:** public protocol / evidence-contract reproduction  
**Private runtime source:** not published  
**Public reproduction of production KORA:** NO

This protocol gives an external reviewer something concrete to run without publishing the private KORA implementation or security-sensitive deployment details.

Its purpose is narrow:

> independently reproduce selected **public verification semantics**, not claim independent reproduction of the private runtime.

## What is reproducible here

The public harness checks a small evidence contract built around the KORA rule:

```text
A terminal state label is not sufficient proof.
```

For a public vector marked `COMPLETED`, the verifier requires:

1. the exact expected evidence IDs;
2. no duplicate evidence IDs;
3. SHA-256-shaped content hashes;
4. a canonical verifier identity for the public example contract;
5. a verifier evidence-set hash recomputed from the evidence records;
6. no failure code on a successful terminal vector.

The canonical evidence-set representation is:

```text
sort by evidence_id
then join each record as:
<evidence_id>:<content_hash>:<provenance>
with newline separators
then SHA-256 the UTF-8 bytes
```

## Included public vectors

`public-evidence/kora-golden-v0.1/golden-public-vectors.json` contains three intentionally synthetic cases:

- **valid_completed** — expected PASS;
- **forged_verifier_hash** — expected FAIL;
- **missing_expected_evidence** — expected FAIL.

They do not come from a private production database and do not disclose private KORA evidence values.

## Run it

Requires Node.js with the standard library only; no npm install is needed.

```bash
node public-evidence/kora-golden-v0.1/verify.mjs
```

Expected final output:

```text
KORA_PUBLIC_VECTOR_SUITE=PASS
```

The command exits non-zero if any vector produces a result different from its declared expectation.

## What this proves

A successful run independently confirms that this public harness enforces the published example semantics for:

- evidence-set completeness;
- terminal-state revalidation;
- verifier-to-evidence binding;
- fail-closed negative vectors.

It is useful because a reviewer can inspect the tiny harness and test vectors directly instead of accepting a screenshot or prose claim.

## What this does NOT prove

This harness does not prove:

- that private KORA source is identical to the public verifier;
- that a private CI result occurred;
- that private evidence is genuine;
- production deployment;
- production database ownership or credentials;
- runtime availability or scale;
- arbitrary/consequential executor safety;
- independent security acceptance;
- pilot reliability;
- product-market fit.

Therefore the correct status remains:

```text
PUBLIC EVIDENCE-CONTRACT REPRODUCTION: AVAILABLE
PRIVATE RUNTIME PUBLICLY REPRODUCED: NO
GOLDEN DEMO RELEASE VERIFIED: NO
```

## External reviewer protocol

An external reviewer can strengthen the evidence by:

1. cloning the public repository at a recorded commit;
2. reviewing `verify.mjs` and the vectors before execution;
3. running the command above in a clean environment;
4. recording Node version, commit SHA, output and timestamp;
5. optionally creating additional negative vectors;
6. reporting discrepancies rather than modifying expected results to force PASS.

A useful independent result should include failures found as well as successes. Generic praise is not evidence.

## Future M6 progression

M6 can move beyond `IN PROGRESS` only after the public package has independent returned evidence and the disclosure boundary has been reviewed.

A future stronger package may add sanitized, signed attestations or additional failure vectors, but private credentials, machine identifiers, internal role names and implementation-sensitive boundaries must remain private.
