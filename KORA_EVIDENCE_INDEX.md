# KORA — Public Evidence Index

This index is the safe entry point for KORA engineering evidence.

- [Public Showcase](KORA_PUBLIC_SHOWCASE.md) — product thesis, high-level architecture and public/private boundary.
- [Engineering Evidence](KORA_ENGINEERING_EVIDENCE.md) — sanitized description of current private CI/test evidence and the M1–M6 acceptance ladder.
- [Golden Demo Scorecard](KORA_GOLDEN_DEMO_SCORECARD.md) — conservative gate status for the Golden Demo.
- [Golden Reproduction Protocol](KORA_GOLDEN_REPRODUCTION_PROTOCOL.md) — public, dependency-free evidence-contract reproduction protocol and claim boundary.
- [`public-evidence/kora-golden-v0.2/`](public-evidence/kora-golden-v0.2/) — current inspectable positive/negative vectors plus the public verifier used by CI.
- [`public-evidence/kora-golden-v0.1/`](public-evidence/kora-golden-v0.1/) — retained earlier public evidence-contract version for provenance/history.
- [Public Golden Demo Architecture](KORA_PUBLIC_GOLDEN_DEMO_ARCHITECTURE.md) — minimum safe reproducible runtime target and acceptance contract.
- [External Technical Review Packet](KORA_EXTERNAL_REVIEW_PACKET.md) — compact review surface for an independent reviewer.
- [Independent Review Template](KORA_REVIEW_REQUEST_TEMPLATE.md) — review protocol to use for external technical review.
- [Pilot Evidence Protocol](KORA_PILOT_METRICS.md) — manual-vs-KORA measurement framework for a low-risk pilot.
- [Evidence Changelog](KORA_PUBLIC_EVIDENCE_CHANGELOG.md) — changes to the public evidence surface.

## Evidence policy

KORA uses the following state distinction:

```text
PREPARED != EXECUTED != VERIFIED != PUBLICLY REPRODUCED
```

Public claims must never outrun the strongest evidence available.

## Current public state

- public evidence-contract harness v0.2: **CI PASS**;
- private runtime publicly reproduced: **NO**;
- independent third-party acceptance: **NO**;
- Golden Demo released: **NO**;
- overall status: **prototype / evidence-building**.

## Versioning rule

The highest public-evidence version listed above is the current reviewer entry point. Earlier versions remain visible for provenance, but should not be treated as the canonical public verifier unless a document explicitly targets that historical version.
