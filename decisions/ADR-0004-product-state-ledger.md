# ADR-0004: Make the versioned product-state ledger canonical

- **Status:** Proposed
- **Date:** 2026-11-02

## Context

AgentRelay must preserve product intent and development continuity across coding agents, workflow engines, repositories and sessions. Treating chat history, an agent's memory store, a task board or a workflow engine's event history as the sole source of truth creates gaps:

- chat and model context are incomplete and provider-specific;
- provider memory may be best-effort, scoped to one ecosystem or subject to retention policies;
- task completion does not establish that a product requirement is satisfied;
- workflow execution state does not prove that an external action succeeded;
- code, CI, PR, deployment and issue state remain authoritative in their respective systems.

W3C PROV-O provides a useful generic model for entities, activities, agents, plans and provenance. Temporal's current agent-state material distinguishes context, long-term memory, execution state and external effect state.

## Decision

AgentRelay should own a **versioned product-state ledger** as the canonical record of the user's approved product intent and the evidence linking that intent to work and outcomes.

The ledger includes goals, requirements, assumptions, architecture decisions, plan versions, work items, approvals, evidence, releases and operational signals. It stores references to external source-of-truth systems rather than copying or replacing all their state.

Agent conversations and provider memories are context sources, not canonical truth. Workflow-engine history is execution recovery data, not the product specification. Telemetry is observability data, not evidence of product correctness by itself.

## Consequences

### Positive
- Project continuity can survive a provider or session change.
- Requirements can be traced to tasks, code, tests, releases and operational feedback.
- Stale or invalidated evidence can be detected.
- Plan changes can be explained and audited.
- Product completion can be separated from task completion.

### Negative
- The data model and synchronization rules are more complex than storing chat summaries.
- External source-of-truth systems can be unavailable or inconsistent.
- Evidence freshness and requirement semantics need careful design.
- The ledger can become a new lock-in point unless export and portability are first-class.
- Provenance and audit records create privacy, retention and access-control obligations.

## Design rules

1. Every requirement, plan, decision and evidence item has a stable ID and version.
2. Every important status claim links to source evidence and the exact code/artifact version it covers.
3. External effects are reconciled against the system that performed them.
4. Evidence is marked stale when its requirement, code, environment or check assumptions change.
5. The manager may propose changes to approved product scope but cannot silently apply them.
6. Provider-specific details and raw event references are retained behind adapters; unsupported capabilities remain explicit.
7. Product state must be exportable in documented, provider-neutral formats.
8. Telemetry content capture is off by default and separate from the product-state ledger.

## Revisit when

- Hands-on competitor tests show that an existing product already provides a sufficiently portable and auditable goal/evidence ledger.
- User research shows developers prefer repository-native files and do not want a separate state service.
- The cost of maintaining cross-system consistency exceeds the measured user value.
