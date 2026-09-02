# Korako Yolawani — Public Evidence Index

> **Current public product identity:** Korako Yolawani  
> **Legacy engineering identifier:** KORA

This index is the safe entry point for public Korako engineering evidence.

Some evidence filenames retain the `KORA_` prefix to preserve historical continuity and avoid rewriting evidence references simply for branding. The canonical public-facing product name is **Korako Yolawani**.

- [Korako public overview](KORAKO.md) — current product thesis, design principles and public/private boundary.
- [Legacy public showcase](KORA_PUBLIC_SHOWCASE.md) — earlier high-level architecture and diligence material.
- [Engineering Evidence](KORA_ENGINEERING_EVIDENCE.md) — sanitized description of current private CI/test evidence and the M1–M6 acceptance ladder.
- [Golden Demo Scorecard](KORA_GOLDEN_DEMO_SCORECARD.md) — conservative gate status for the Golden Demo.
- [Golden Reproduction Protocol](KORA_GOLDEN_REPRODUCTION_PROTOCOL.md) — public, dependency-free evidence-contract reproduction protocol and claim boundary.
- [`public-evidence/kora-golden-v0.1/`](public-evidence/kora-golden-v0.1/) — inspectable positive/negative vectors plus the public verifier used by CI.
- [Independent Review Template](KORA_REVIEW_REQUEST_TEMPLATE.md) — review protocol for external technical review.
- [Pilot Evidence Protocol](KORA_PILOT_METRICS.md) — manual-vs-Korako measurement framework for a low-risk pilot.
- [Evidence Changelog](KORA_PUBLIC_EVIDENCE_CHANGELOG.md) — changes to the public evidence surface.

## Evidence policy

Korako uses the following state distinction:

```text
PREPARED != EXECUTED != VERIFIED != PUBLICLY REPRODUCED
```

Public claims must never outrun the strongest evidence available.

## Current public state

- public evidence-contract harness: **CI PASS**;
- private runtime publicly reproduced: **NO**;
- independent third-party acceptance: **NO**;
- Golden Demo released: **NO**;
- overall status: **prototype / evidence-building**.

## Interpretation

A green public evidence check means the published evidence contract passed the checks defined for that artifact. It is **not** a claim that the private runtime, full product, security model or future integrations have been independently validated.

That distinction is intentional: **evidence before scale, and narrower claims before broader claims.**
