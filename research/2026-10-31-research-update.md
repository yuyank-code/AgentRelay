# Research Update — 2026-10-31

**Run date:** 2026-10-31  
**Scope:** Product reframing, market/competitor scan, frameworks and protocols, agent integration, feasibility, verification, and evaluation.

## 1. Most important product correction

The repository's initial framing described AgentRelay mainly as a cross-agent handoff layer. The current product vision is broader:

**AgentRelay should be an AI development manager that takes a software goal and continuously manages the work needed to reach a verified product outcome.**

The user is the product owner. AgentRelay manages discovery, requirements, architecture, planning, agent coordination, verification, replanning, release preparation, and eventually maintenance. Coding agents and tools are the workforce.

Handoff and project continuity remain foundational, but they are not the complete product. A successful task does not mean the product goal has been achieved.

The intended manager loop is:

**Understand goal → assess current state → choose next action → check policy → execute → verify evidence → update project state → continue or escalate.**

This is recorded in [product-vision.md](../docs/product-vision.md) and reflected in the revised [product brief](../docs/product-brief.md) and [architecture](../architecture/initial-architecture.md).

## 2. Market finding: the feature space is crowded

A scan of official product pages and documentation found substantial overlap:

- GitHub Copilot supports asynchronous coding tasks from issues and prompts, PR creation, follow-up iteration, and third-party Claude/Codex agents in public preview.
- Factory positions Droids as a model-independent system that plans, builds, reviews, and ships, with memory, adjustable autonomy, mission control, analytics, and automations.
- Shep advertises local-first multi-agent execution across many coding CLIs, isolated worktrees, optional spec/plan approval, CI, review, PRs, and human triage.
- CallCode advertises request decomposition, parallel agents, and use of existing coding-tool subscriptions.
- Proliferate advertises isolated coding-agent workspaces, review gates, integrations, and recurring/event-driven automations.
- CodeMerlin positions around specification, planning, implementation, and review governance.
- Specto focuses on idea-to-PRD/technical-spec/architecture/sprint-plan workflows.
- Devin offers autonomous coding sessions and code/test/PR workflows.
- Bolt offers a direct idea-to-full-stack-app-and-deployment experience.
- ServiceNow's 2026-10-06 AI Workflow Factory announcement describes a governed, continuous outcome-oriented workflow loop with autonomous planning, building, testing, and third-party agent integration.

**Conclusion:** do not claim that adapters, memory, routing, worktrees, task graphs, review, approvals, or generic idea-to-code are unique. Public product claims need hands-on verification, but they are sufficient to invalidate a simple feature-list moat.

The possible opening is a more specific outcome: a portable, auditable project model that links the original product goal to requirements, decisions, implementation, verification, releases, and runtime feedback, and uses that evidence to decide what should happen next. This is only a hypothesis; Factory and ServiceNow already claim broad outcome-driven systems.

See [competitive-landscape.md](competitive-landscape.md).

## 3. Framework and protocol findings

- **LangGraph:** official docs describe checkpointed state, human interrupts, memory, time travel, and fault-tolerant resumption. Useful for agentic reasoning, but not a substitute for AgentRelay's product data model.
- **Microsoft Agent Framework:** Microsoft announced 1.0 GA on 2026-04-02 as the convergence of AutoGen and Semantic Kernel. The 2026-06-03 announcement describes an agent harness with tools, context, approval, background agents, and observability. Evaluate current MAF before choosing older AutoGen-specific patterns.
- **AutoGen:** agent/team state can be saved and loaded. This is agent orchestration state, not automatically a durable project/evidence ledger.
- **CrewAI:** Flows provide structured state, conditional execution, and persisted workflow state; Crews can be used for delegated work. This is an alternative implementation framework, not the product itself.
- **Google ADK 2.0:** graph-based, dynamic, and collaborative workflows are documented across Python, TypeScript, and Go. Another framework alternative.
- **OpenHands SDK:** Python, TypeScript, and REST APIs with local or ephemeral Docker/Kubernetes workspaces. A promising execution runtime.
- **A2A:** Agent Cards, tasks, messages, artifacts, context IDs, polling, streaming, and push notifications. It handles agent interoperability, not project management or software verification.
- **MCP:** resources, prompts, tools, progress, cancellation, error reporting, and logging. It connects agents to tools/context, not agents to each other or projects to outcomes.
- **Temporal:** durable event-history-based workflow execution and replay for long-running processes. Consider if AgentRelay needs reliable multi-day workflows and external signals.

**Recommendation:** own the product model; choose one workflow engine; use MCP for tools and A2A for remote agents when needed. Do not start with LangGraph + AutoGen + CrewAI + ADK + Temporal simultaneously.

See [frameworks-and-protocols.md](frameworks-and-protocols.md).

## 4. Agent integration findings

Technically, several agent runtimes have usable programmatic surfaces:

- Claude Agent SDK supports Python/TypeScript and exposes the Claude Code agent loop, tools, permissions, sessions, hooks, subagents, MCP, and structured output.
- Codex CLI documents repeatable automation through codex exec and local repository workflows.
- Gemini CLI headless mode supports JSON and JSONL output, event records, usage/latency statistics, and exit codes.
- Gemini API CLI can invoke API-managed agents from shell workflows, but is not itself a coding-agent runtime.
- OpenHands provides Python/TypeScript/REST interfaces and local or ephemeral workspaces.
- GitHub offers hosted Copilot agent workflows and third-party Claude/Codex integrations.

### Critical Claude subscription constraint

The current Anthropic Agent SDK documentation says third-party developers must not offer Claude.ai login or subscription rate limits for their products unless approved, and directs product builders to API-key authentication. The current API credit help page says Pro is not eligible for monthly API credits; Max, Ultra, and Team are eligible. It distinguishes API-key-backed SDK use from usage signed in with a Claude plan.

**Implication:** do not design AgentRelay as a third-party product that relies on piggybacking a user's Claude Pro credentials or reselling their usage limits. The builder may use Claude as a development tool, but AgentRelay's own provider integration must use a permitted auth/billing route. Validate the exact terms for any local user-operated CLI mode before distributing it.

See [agent-integration-feasibility.md](agent-integration-feasibility.md).

## 5. Verification and long-horizon evaluation

Two 2026 research directions are especially relevant:
- ProgramBench (submitted 2026-05-05) studies holistic program reconstruction and reports that none of nine evaluated models fully solved any of its 200 tasks in the paper's setting.
- SWE-STEPS (submitted 2026-04-03) evaluates chains of dependent PRs and reports that isolated PR evaluations can overstate performance by ignoring spillover and repository-health effects.

OpenAI's 2026-02-23 analysis also argues SWE-bench Verified is increasingly contaminated and recommends SWE-bench Pro for frontier coding evaluation.

These sources support a key design choice: build a sequential, project-level evaluation harness rather than relying on a single benchmark score or the agent's self-report.

See [evaluation-plan.md](evaluation-plan.md).

## 6. Architecture implications

The architecture should be split into:
- **Control plane:** product goal, requirements, decisions, project graph, planner, policy, approvals, evidence, progress, and user experience.
- **Execution plane:** coding agents, tools, isolated workspaces, Git, tests, browser automation, CI, preview environments, deployment, and monitoring.

The key data relationship is:

Goal → requirement → acceptance check → task → agent run → commit/diff → evidence → release → runtime feedback.

The LLM proposes and reasons; deterministic policy authorizes; workflow infrastructure persists; execution tools enforce workspace boundaries.

## 7. Recommended first vertical slice

Do not begin by building a generic multi-agent marketplace or by connecting five frameworks.

Build a small but complete goal-to-evidence workflow:
1. user enters a software idea;
2. AgentRelay asks clarifying questions;
3. creates a project charter, requirements, and measurable acceptance checks;
4. produces a dependency-aware plan;
5. dispatches work to one supported coding agent in an isolated workspace;
6. captures diffs, tests, logs, and preview behavior;
7. assesses requirement coverage and identifies gaps;
8. replans or retries within bounded limits;
9. asks for human approval for important decisions;
10. continues to the next task and presents the updated project status.

This is still a narrow experiment, but it tests the product's actual thesis rather than only proving that two agents can exchange prompts.

## 8. Important unresolved questions

- Which customer segment has the strongest unmet pain: solo builders, small teams, or enterprise teams?
- Can AgentRelay demonstrate better goal completion than a single strong coding agent with good project instructions?
- What is the minimum independent evidence needed to mark a requirement verified?
- How much autonomy do users actually want?
- Which integrations are stable and permitted for third-party products?
- Should the initial runtime be local-first, cloud-hosted, or hybrid?
- Does the product need Temporal, or can a simpler state machine/LangGraph handle the first year?
- What is the measurable advantage over Factory, Shep, GitHub Copilot, Devin, and direct app builders?

## 9. Sources reviewed on 2026-10-31

- GitHub: [third-party coding agents](https://docs.github.com/en/copilot/concepts/agent-capabilities/about-agents/about-third-party-coding-agents), [Copilot on GitHub.com](https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/copilot-surfaces/copilot-on-github)
- Anthropic: [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview), [monthly API credits and SDK use](https://support.claude.com/en/articles/17154008-monthly-api-credits-for-max-ultra-and-team-plans)
- OpenAI: [Codex CLI](https://developers.openai.com/codex/cli), [SWE-bench Verified analysis](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/)
- Google: [Gemini CLI headless mode](https://geminicli.com/docs/cli/headless/), [Gemini API CLI](https://ai.google.dev/gemini-api/docs/cli), [ADK 2.0](https://github.com/google/adk-docs/blob/main/docs/2.0/index.md)
- LangChain: [LangGraph checkpointers](https://docs.langchain.com/oss/python/langgraph/persistence)
- Microsoft: [Agent Framework at BUILD 2026](https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-at-build-2026-announce/), [AutoGen state management](https://microsoft.github.io/autogen/dev/user-guide/agentchat-user-guide/tutorial/state.html)
- CrewAI: [Flow state management](https://github.com/crewAIInc/crewAI/blob/main/docs/v1.10.1/en/guides/flows/mastering-flow-state.mdx)
- OpenHands: [Software Agent SDK](https://github.com/OpenHands/software-agent-sdk)
- A2A: [core concepts](https://a2a-protocol.org/latest/topics/key-concepts/)
- MCP: [2025-11-25 specification](https://modelcontextprotocol.io/specification/2025-11-25)
- Temporal: [workflow documentation](https://docs.temporal.io/workflows)
- Research: [ProgramBench, arXiv:2605.03546](https://arxiv.org/abs/2605.03546), [SWE-STEPS, arXiv:2604.03035](https://arxiv.org/abs/2604.03035)
- Competitors: [Factory](https://factory.com/), [Shep](https://shep.bot/), [CallCode](https://callcode.dev/), [Proliferate](https://www.proliferate.dev/), [CodeMerlin](https://www.codemerlin.ai/), [Specto](https://specto.codes/), [Devin](https://devin.ai/), [Bolt](https://bolt.new/use-cases/ai-app-builder), [ServiceNow announcement](https://newsroom.servicenow.com/press-releases/details/2026/ServiceNow-launches-AI-Workflow-Factory-to-turn-workflow-improvement-into-a-continuous-agentic-AI-powered-loop/default.aspx)
