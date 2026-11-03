# ADR-0005: Treat AgentRelay Differentiation as Unproven Until Validated

- **Status:** Proposed
- **Date:** 2026-11-03

## Context

The original differentiation hypotheses were persistent project continuity, evidence-based handoffs, coding-environment adapters, policy controls, and a unified developer experience.

The current public competitive landscape substantially overlaps with these capabilities:
- Factory describes a model-independent Software Factory spanning planning, multi-agent execution, validation, release, cross-system integrations, persistent context, and operational signals.
- Shep describes a local-first, multi-provider workflow from app idea/spec through worktrees, checkpoints, CI repair, approvals, and merge.
- GitHub Agentic Workflows documents dynamic workflows, iterative feature growth, research-plan-assign, project operations, cross-run memory, a Git-backed ledger, and safe outputs/security.
- Provider harnesses increasingly supply sessions, context compaction, approvals, recovery, and sandbox execution.

A feature checklist or architectural novelty claim is not enough to justify a standalone product.

## Decision

Treat AgentRelay's market differentiation as **unproven**. Do not expand into a broad agent orchestrator before:
1. hands-on comparison against the strongest relevant products;
2. interviews based on users' recent real workflows;
3. concierge pilots measuring the value of a product-goal/evidence ledger;
4. a controlled test showing improvement in verified product outcomes, human coordination, cost, or portability.

The candidate wedge to test is a user-owned, provider- and tool-neutral product-goal control plane that connects approved goals and requirements to current acceptance evidence, implementation artifacts, releases, and operational feedback across systems.

This is a hypothesis, not a claim of uniqueness. If existing products deliver comparable outcomes with similar effort, prefer an integration/extension or a narrower open-source component rather than competing as another full-stack orchestrator.

## Consequences

### Positive
- Reduces the risk of building capabilities already commoditized.
- Forces product claims to be tied to observed user value.
- Encourages reuse of existing agent and workflow infrastructure.
- Makes no-go or pivot decisions legitimate outcomes of research.

### Negative
- Slows implementation while validation is performed.
- Requires access to competitor products and representative users.
- May reveal that the standalone product concept is not differentiated.

## Required evidence to revisit

- A reproducible side-by-side benchmark with comparable goals, models where possible, budgets, permissions, and human time.
- Measured reduction in human coordination or improvement in verified outcomes without safety/quality regression.
- Repeated user evidence that cross-provider, cross-system product continuity is a painful unsolved job.
- Clear explanation of why an integration with Factory, Shep, GitHub, or provider-native tools would not solve the same problem.

## Sources

- [Factory Software Factory](https://factory.com/)
- [Factory Missions](https://factory.com/product/missions)
- [Shep](https://shep.bot/)
- [GitHub Copilot dynamic workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/dynamic-workflows)
- [GitHub Agentic Workflows Feature Grower](https://github.github.com/gh-aw/patterns/feature-grower/)
- [GitHub Agentic Workflows ResearchPlanAssignOps](https://github.github.com/gh-aw/patterns/research-plan-assign-ops/)
- [GitHub Agentic Workflows ProjectOps](https://github.github.com/gh-aw/patterns/project-ops/)
- [GitHub Agentic Workflows Git-backed Ledger](https://github.github.com/gh-aw/specs/repo-memory-ledger-specification/)
- [OpenAI Agents API](https://developers.openai.com/api/docs/guides/agents-api/overview)
