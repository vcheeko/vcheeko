# KORA — Pilot Evidence Protocol

Run this only after the Golden Demo is internally verified and has received independent technical review appropriate to the pilot risk.

## Goal

Measure whether KORA reduces operational burden **without hiding risk or producing incorrect state transitions**.

## Compare the same workflow

### Baseline — manual AI/tool workflow

Capture:

- total elapsed time;
- number of user interactions;
- manual copy/paste or transfer steps;
- tools/windows used;
- dependency/order mistakes;
- incorrect completion/state claims;
- recovery time after failure;
- consequential actions with verification evidence.

### KORA-assisted workflow

Capture the same fields plus:

- Human Gate prompts;
- blocked actions;
- verifier PASS/FAIL counts;
- retries;
- recovery events;
- state transitions;
- evidence references.

## Core metrics

| Metric | Baseline | KORA | Change |
| --- | ---: | ---: | ---: |
| Elapsed time |  |  |  |
| User interactions |  |  |  |
| Manual transfers/copy-paste |  |  |  |
| Incorrect state transitions |  |  |  |
| Missed dependencies |  |  |  |
| Recovery success |  |  |  |
| Consequential actions with verification evidence |  |  |  |

## Integrity rules

- Use the same task definition and acceptance criteria.
- Record failed runs; do not report only the best run.
- Do not count `EXECUTED` as `VERIFIED`.
- Report human interventions.
- Report safety blocks even when they make KORA slower.
- Publish known limitations and sample size.

## Minimum useful pilot evidence

A pilot result should include:

- workflow definition;
- number of runs;
- baseline method;
- KORA version/commit;
- acceptance criteria;
- aggregate results;
- failures/outliers;
- conclusion limited to what the data actually supports.

**The objective is not to prove KORA wins every metric. The objective is to measure whether governed automation improves the workflow reliably.**
