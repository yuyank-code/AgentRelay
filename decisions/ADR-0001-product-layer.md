# ADR-0001: Build a product layer, not another orchestration framework

- **Status:** Proposed
- **Date:** 2026-10-30
- **Reaffirmed:** 2026-10-31

## Context

AgentRelay's initial concept was to coordinate work between different AI models and coding environments. The product vision has since been clarified: AgentRelay should manage software development from a product goal through delivery and improvement, not merely route prompts or hand off coding tasks.

Frameworks such as LangGraph, Microsoft Agent Framework, CrewAI, and Google ADK already provide agent/workflow composition and state features. Temporal provides durable workflow execution. A2A provides agent-to-agent interoperability, while MCP standardizes agent-to-tool/context connections. OpenHands provides a software-agent runtime.

Reimplementing these generic capabilities would create a large maintenance burden and weak differentiation.

## Decision

AgentRelay should own the product-level domain model and user experience for goal-oriented software development, while integrating existing orchestration frameworks, runtimes, and protocols where they fit.

The product-owned layer should focus on:
- product goals, requirements, assumptions, and success criteria;
- versioned architecture and decision records;
- dependency-aware plans and dynamic replanning;
- project continuity independent of provider conversation history;
- repository-grounded context and provenance;
- coding-agent adapters and capability metadata;
- execution evidence and requirement-level verification;
- release and operational feedback links;
- policy, approvals, escalation, and auditability;
- a clear developer experience across tools and lifecycle stages.

## Consequences

### Positive
- Avoids rebuilding generic agent frameworks.
- Keeps product state portable across providers and sessions.
- Differentiation is tied to product outcomes and evidence rather than agent routing.
- Allows the workflow engine and agent runtime to change independently.

### Negative
- The product model and evidence graph are difficult to design correctly.
- Integrations and adapters require ongoing maintenance.
- Existing products already overlap substantially.
- Goal-level verification is much harder than task-level orchestration.
- A broad lifecycle product requires careful staged scope.

## Revisit when

- A framework or commercial product provides the required product-goal and evidence lifecycle in a way that removes the need for AgentRelay.
- The proposed product-owned layer proves commoditized.
- User research identifies a narrower, more valuable wedge.
- Durable execution, policy, or verification requirements force a different architecture.
