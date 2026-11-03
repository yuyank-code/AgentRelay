# AgentRelay Research Program

**Started:** 2026-10-30  
**Latest research update:** 2026-11-02  
**Status:** Active

## Current product framing

AgentRelay is being investigated as an **AI development manager** that can drive a software product from idea toward release and improvement. The user is the product owner; AgentRelay manages the day-to-day development loop; existing AI agents and tools perform bounded work.

Cross-agent handoff remains important, but it is a supporting capability rather than the entire product. The durable object is the product goal and its linked requirements, decisions, tasks, evidence, releases, and feedback.

## Research questions

1. Is there a validated, recurring pain around developers manually managing AI-driven product development?
2. Which existing products already cover idea-to-product planning, multi-agent coding, verification, release, and governance?
3. What product-level responsibility remains meaningfully underserved?
4. Which agents expose supported SDK, CLI, API, hosted, or protocol interfaces?
5. How should product goals, requirements, decisions, plans, and evidence be versioned?
6. How can AgentRelay know whether the product is progressing toward the original goal?
7. How can verification be independent enough to catch false completion claims?
8. How should plans change when requirements, architecture, dependencies, or runtime feedback change?
9. What level of autonomy can be safe, affordable, and useful?
10. What benchmark can measure sequential project outcomes rather than isolated coding tasks?
11. Which architecture should own durable workflows, and which capabilities should be delegated to frameworks?
12. What wedge, customer segment, and business model could survive fast-moving platform competition?

## Research standards

- Prefer primary documentation, specifications, source repositories, changelogs, and dated announcements.
- Record access/review dates and distinguish documented facts, vendor claims, and hypotheses.
- Verify whether a capability is supported by an official interface, a community wrapper, or fragile UI automation.
- Compare end-to-end workflows, not just feature checklists.
- Record negative findings and competing products.
- Do not treat a model-generated answer or self-review as independent verification.
- Do not copy proprietary documentation or long source passages.
- Treat vendor marketing as a statement of positioning, not proof of product performance.
- Revisit fast-changing integration, pricing, authentication, and terms details before implementation.

## Current documents

- [Product vision](../docs/product-vision.md)
- [Product brief](../docs/product-brief.md)
- [Initial architecture](../architecture/initial-architecture.md)
- [Competitive landscape](competitive-landscape.md)
- [Frameworks and protocols](frameworks-and-protocols.md)
- [Agent integration feasibility](agent-integration-feasibility.md)
- [2026-10-31 research update](2026-10-31-research-update.md)
- [2026-11-01 research update](2026-11-01-research-update.md)
- [2026-11-02 research update](2026-11-02-research-update.md)
- [Evaluation plan](evaluation-plan.md)
- [Security and trust](../docs/security-and-trust.md)
- [Roadmap](../roadmap/roadmap.md)
- [ADR-0001](../decisions/ADR-0001-product-layer.md)
- [ADR-0002](../decisions/ADR-0002-goal-oriented-manager.md)

## Current hypothesis

The strongest possible product layer is not generic agent routing. It is **goal-to-evidence lifecycle management**: preserve the user's intended outcome, convert it into versioned requirements and plans, coordinate existing agents and tools, connect actual work to reproducible evidence, replan based on the evidence, and keep humans in control of consequential decisions.

This is not yet a proven market gap. Factory, Shep, GitHub Copilot, and other products already cover significant portions of agent coordination, planning, memory, review, autonomy, and governance. Research must continue to challenge this hypothesis.

## Publishing history

- 2026-10-30: initial product and architecture documents created.
- 2026-10-31: reframed the product around a goal-oriented AI development manager; added current competitor, framework, integration, benchmark, and product-risk research. This run's findings are consolidated in one research update and published in one Git commit.
- 2026-11-01: refreshed the competitor analysis against Shep, GitHub Agentic Workflows, Factory, OpenHands Automation, and Codex's embeddable harness; corrected the nuanced Anthropic subscription/auth finding; clarified the manager-versus-harness boundary. Published together in one Git commit.


## New findings from 2026-11-01

The strongest competitor pressure is now clearly visible in Shep's spec-driven feature workflow, GitHub's multi-agent and Agentic Workflows surfaces, and Factory's cross-model prompt-to-PR positioning. Generic agent routing, plan generation, approvals, retries, worktrees, and repository automation should be treated as table stakes.

The architectural response is to keep AgentRelay's ownership at the product/control-plane layer and reuse provider-native agent harnesses or OpenHands for execution. The possible wedge remains long-lived product-goal traceability across requirements, implementation, release, and operational feedback, but it is not yet validated.

Anthropic's current Agent SDK overview and Help Center subscription article need to be interpreted together; their wording creates a product-distribution permission question. The repository now records this nuance rather than making a blanket claim about Claude plan authentication.


## New findings from 2026-11-02

GitHub Copilot dynamic workflows document sequential/parallel agent orchestration, structured handoffs, subagent verification, checkpoints, pause/resume and budgets. GitHub Agentic Workflows support multiple coding-agent engines with repository guardrails. Copilot Memory spans several GitHub surfaces, although it remains an ecosystem-scoped memory mechanism rather than a provider-neutral product-state ledger. Google's Jules exposes plan approval, task sessions, activities, follow-up messages and an alpha REST API.

These findings further commoditize generic orchestration, memory, approvals and agent selection. The remaining hypothesis is a portable, versioned product-state/evidence layer connecting the original goal to requirements, decisions, code, tests, releases and later feedback. Public docs do not prove competitors lack this; hands-on tests are required.

A recent Reddit discussion provides a qualitative example of a solo/non-developer builder coordinating a master design document, ticket system, scheduled Codex implementation, Claude review and user testing. Other commenters describe wiki/ticket ingestion, deterministic gates, worktrees and independent review. This is evidence of DIY workflows, not representative market validation.

Architecture research now separates product/domain state, execution state, external effect state, agent memory/context and observability. W3C PROV-O offers useful provenance semantics; Temporal can provide durable execution where justified; OpenTelemetry GenAI conventions are still in Development and warn against capturing sensitive prompts/tool payloads by default.

## Publishing history — 2026-11-02

Added the product-state/evidence model and ADR-0004; refreshed competitor and integration research for GitHub Copilot dynamic workflows, Agentic Workflows, Copilot Memory and Jules; added user-workflow signals, a concierge validation experiment, Temporal/provenance guidance and telemetry privacy controls. All changes from this run are published together in one Git commit.

## New findings from 2026-11-03

The competitive stress test materially weakens the original differentiation hypothesis:
- Factory's current Software Factory and Missions positioning covers model-independent, multi-agent development across planning, implementation, validation, release, integrations, operational signals and multi-repository work.
- Shep v1.253 documents local-first multi-provider execution, idea-to-app startup, spec/requirements planning, worktrees, per-step checkpoints, approvals, CI auto-fix retries and human-controlled merge.
- GitHub Agentic Workflows documents dynamic workflows, iterative Feature Grower, ResearchPlanAssignOps, ProjectOps, MemoryOps, a Git-backed Ledger working draft, and a layered security architecture.
- OpenAI's Agents API provides a managed Codex harness with session orchestration, context compaction, recovery and hosted/self-hosted execution options.

Feature-level differentiators (agent adapters, memory, task planning, parallel execution, approvals, CI recovery) are not defensible alone. The only candidate to test is a portable, semantically typed product-goal/evidence control plane across providers, code hosts and lifecycle systems. Factory already approaches this claim in its public positioning, so no market gap is established.

The next gate is hands-on competitor benchmarking plus 5–10 problem interviews and 3–5 concierge pilots. Prefer integrating existing execution substrates until evidence shows a standalone runtime is necessary.

Research sources and conclusions are recorded in [2026-11-03 research update](2026-11-03-research-update.md).
