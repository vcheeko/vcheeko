# KORA — Public Showcase

> **A human-directed operating layer for reliable AI-assisted work.**

**Stage:** prototype / evidence-building  
**Production-ready:** no  
**Public-source implementation:** no — security-sensitive and canonical engineering remain private

This document is deliberately a **public diligence surface**, not a source-code release. It explains what KORA is trying to solve, how the system is structured at a high level, what has been exercised internally, and what is still unproven.

## The problem

Capable AI can perform individual tasks, but serious multi-step work still pushes a large operational burden back onto the user:

- remembering state across sessions;
- choosing the right tool or agent;
- respecting dependencies;
- knowing when approval is required;
- distinguishing "requested" from "actually executed";
- verifying that execution succeeded;
- recovering after failure;
- coordinating parallel work without corrupting canonical state.

KORA explores an operating layer between **human intent** and **AI/tool execution**.

## Core hypothesis

A reliable AI operating layer should combine:

1. **Natural-language control** — goals first, not command memorization.
2. **Persistent project state** — done, active, blocked, next and why.
3. **Dependency-aware planning** — work proceeds only when prerequisites are satisfied.
4. **Risk-proportional authority** — low-risk work can flow; consequential work gets stronger gates.
5. **Tool/model neutrality** — models and tools are replaceable capabilities, not the identity of the system.
6. **Verification before trust** — important completion claims require evidence.
7. **Recovery-aware execution** — reversible actions, checkpoints and explicit failure states.
8. **Human authority** — the system can coordinate work without silently taking ownership of consequential decisions.

## High-level architecture

```mermaid
flowchart TD
    H[Human goal] --> I[Intent + project state]
    I --> P[Plan + dependencies]
    P --> A[Risk / authority decision]
    A --> O[Tool or agent orchestration]
    O --> E[Bounded execution]
    E --> V[Verification + evidence]
    V --> S[State update]
    S --> N[Next valid action / continuation]
    V -->|failure or insufficient evidence| R[Recovery / blocked state]
    R --> S
```

A useful KORA interaction should feel much simpler than this diagram: the complexity belongs behind the interface, while the person sees the **current state, next valid action, meaningful blockers and approvals that actually matter**.

## Prototype evidence boundary

Private development has implemented or substantially exercised prototype work around:

- versioned governance and human-authority rules;
- controlled and fail-closed workflow behavior;
- request → authorization → execution → verification → state-update semantics;
- review and evidence workflows;
- read-only and simulation-oriented validation paths;
- safe-parallelism rules for independent, reversible work;
- bounded local execution / bridge experiments;
- recovery and continuation concepts.

These statements describe **internal prototype evidence**, not independent public verification. This showcase intentionally does not publish implementation details, machine-specific configuration, credentials, exact security boundaries, internal hashes or operational evidence chains.

A reviewer should therefore treat the items above as **claims backed by private project evidence, not as publicly reproduced results**.

## What KORA is not

KORA is not currently:

- a foundation model;
- a production-ready autonomous agent platform;
- a replacement for human decision-making;
- a claim that every workflow should be automated;
- a finished commercial product;
- proof of product-market fit;
- proof that private prototype results generalize to production environments.

## Current engineering target

The most important milestone is a small end-to-end core that can repeatedly demonstrate:

```text
GOAL
  → PLAN
  → AUTHORIZED ACTION
  → EXECUTION
  → VERIFICATION
  → PERSISTENT CONTINUATION
```

The standard is not "the model said it completed the task." The standard is that the system can show **what happened, what evidence supports the state change, what remains uncertain, and what the next safe action is**.

## Human / AI responsibility split

KORA is being developed with extensive AI assistance. That is disclosed rather than hidden.

**Miha Tavčar:** product vision, systems thinking, workflow/governance design, acceptance criteria, AI-assisted prototyping, testing direction and project decisions.

**AI tools/models:** research assistance, implementation assistance, critique, documentation, test generation and review support where appropriate.

**Required discipline:** AI output is not automatically accepted as truth or completion. Important claims should be challenged through tests, evidence, independent review or explicit human judgment proportional to consequence.

## Public / private boundary

### Safe to discuss publicly

- problem and product thesis;
- high-level architecture;
- design principles;
- prototype stage and known limitations;
- collaboration needs;
- non-sensitive demonstration results when deliberately released.

### Private by design

- canonical internal rule registers;
- detailed execution implementation;
- exact approval and security boundaries;
- credentials or machine-specific configuration;
- private operational logs and evidence chains;
- unpublished IP-sensitive material.

## What would materially increase confidence

The next credibility upgrades are intentionally measurable:

1. a reproducible public Golden Demo using non-sensitive tasks;
2. visible acceptance tests and pass/fail criteria;
3. a small public evaluation set comparing KORA-assisted work with a manual baseline;
4. independent technical review of architecture and failure handling;
5. documented recovery from deliberately injected failures;
6. pilot evidence showing reduced user burden without hiding risk.

## Collaboration

KORA would benefit most from:

- a technical co-founder / lead engineer with strong backend and systems judgment;
- reliability and security engineering review;
- AI orchestration and evaluation expertise;
- product/UX work focused on making governed automation feel simple;
- pilot partners willing to define measurable real-world workflows.

---

**The ambition is large; the public claims are intentionally narrow. Evidence before scale.**
