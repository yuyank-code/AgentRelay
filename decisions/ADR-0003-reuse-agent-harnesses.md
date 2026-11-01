# ADR-0003: Reuse agent harnesses; own the product-control plane

- **Status:** Proposed
- **Date:** 2026-11-01

## Context

AgentRelay's ambition is to manage software development from a product goal toward a verified outcome. It needs coding and research agents, tools, workspaces, streaming, sessions, and permission handling.

Current official ecosystems already expose substantial execution infrastructure:
- Claude Agent SDK exposes the Claude Code agent loop, tools, sessions, hooks, permissions, subagents, and observability.
- Codex provides CLI, SDK, and app-server integration for bounded jobs and persistent application sessions.
- OpenHands provides an SDK/Agent Server and a separate automation service for scheduled/event-driven dispatch.
- GitHub Agentic Workflows runs several coding agents inside guarded repository automations.

Rebuilding these harness capabilities would duplicate work, increase security risk, and distract from the product-level manager problem.

## Decision

AgentRelay will be designed primarily as a **product-development control plane**, not as a new general-purpose coding-agent harness.

AgentRelay owns:
1. the original product goal, approved scope, users, and success measures;
2. versioned requirements, assumptions, constraints, and architecture decisions;
3. roadmap, dependencies, task selection, and plan revisions;
4. cross-provider continuity and task-specific context compilation;
5. requirement-level verification, evidence provenance, and release state;
6. policy, budgets, approval, escalation, and audit;
7. eventual links to operational feedback and maintenance work.

Provider-native SDKs, CLIs, app servers, hosted agents, or OpenHands may own the model/tool loop and execution workspace. AgentRelay adapters expose capabilities and translate lifecycle events without pretending all providers are equivalent.

## Design rules

- The manager's internal plan is separate from any agent's internal task list.
- The model may recommend an action; a deterministic policy layer authorizes it.
- Raw provider events and artifacts remain available alongside normalized AgentRelay events.
- An adapter must declare unsupported capabilities explicitly.
- AgentRelay must not infer product completion from an agent's final response or a successful task.
- User-owned authentication, provider billing, and provider terms must be explicit and replaceable.
- Do not collect or proxy provider passwords, browser cookies, or subscription credentials.
- Add a custom execution runtime only when measurable requirements cannot be met by available harnesses.

## Consequences

### Positive
- More engineering effort can focus on product-level goal state, requirements, evidence, and decision quality.
- Existing provider work on tool loops, sessions, and sandboxing can be reused.
- AgentRelay can support different execution models without forcing every provider into one identical interface.

### Negative
- Provider APIs, SDKs, CLI output, and terms may change.
- The lowest common denominator may be too weak for some workflows; capability-aware routing and raw event retention are required.
- Reusing a provider harness creates some operational and product dependency on that provider.
- Different authentication and billing models complicate a consistent user experience.

## Revisit when

- No supported harness can meet an essential execution/security requirement.
- Adapter maintenance costs exceed the cost of a controlled runtime.
- A provider's terms or API changes make the integration unavailable.
- User research shows that AgentRelay's actual value requires a deeply integrated execution environment rather than a control plane.
