# ADR-0001: Build a product layer, not another orchestration framework

- **Status:** Proposed
- **Date:** 2026-10-30

## Context

AgentRelay's initial concept is to coordinate work between different AI models and coding environments. Frameworks such as LangGraph already provide stateful orchestration, branching, persistence, and human-in-the-loop primitives. A2A provides an interoperability protocol for agent-to-agent communication, while MCP standardizes agent-to-tool connections.

Reimplementing generic orchestration or agent messaging would create a large maintenance burden and weak differentiation.

## Decision

AgentRelay should own the product-level domain model for software project continuity and handoff, while integrating existing orchestration frameworks and protocols when useful.

The product-owned layer should focus on:
- project/task state independent of provider chat sessions;
- repository-grounded context and provenance;
- explicit handoff contracts;
- coding-agent adapters and capability metadata;
- execution evidence and verification;
- workspace isolation and conflict handling;
- policy, approvals, and auditability;
- developer-facing continuity across agents.

## Consequences

### Positive
- Can use mature orchestration frameworks instead of rebuilding them.
- Differentiation is tied to developer workflow outcomes.
- Provider-neutral project state can outlive any one agent session.
- Easier to add or replace orchestration technology.

### Negative
- Integrations and adapters still require ongoing maintenance.
- Product must carefully normalize different agent capabilities without hiding important differences.
- Project-state and evidence models require thoughtful design.
- Existing coding tools may close the gap over time.

## Revisit when

- A framework cannot support required pause/resume, durable execution, or policy semantics.
- A2A or another standard evolves to cover most of the proposed handoff lifecycle.
- Competitor research shows the product-owned layer is already commoditized.
