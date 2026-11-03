# AgentRelay Research Update — 2026-11-03

**Research date:** 2026-11-03  
**Scope:** cumulative product/engineering research; competitive differentiation stress test, workflow patterns, execution substrates, state integrity, security, evaluation, and next product gate.  
**Evidence standard:** vendor documentation is evidence of documented features and vendor claims, not independent proof of quality or adoption. Academic findings are summarized with their sample and limitations. No hands-on competitor trials or user interviews were completed in this run.

## Executive conclusion

The most important finding is negative but useful: **the current AgentRelay differentiation hypothesis is not yet defensible.**

The broad concept—an AI manager that accepts a software goal, plans work, coordinates coding agents, preserves memory, reviews progress, recovers from failures, and moves toward release—is already closely approached by several products and documented workflows.

Three competitive developments materially change the assessment:

1. **Factory's Software Factory** publicly describes model-independent planning, execution, validation, release, integrations, persistent context, analytics, operational signals, and multi-agent Missions.
2. **Shep v1.253** publicly describes a local-first, MIT-licensed, multi-provider workflow from app idea and spec through isolated agent worktrees, checkpoints, approvals, CI fixes, and human-controlled merge.
3. **GitHub Agentic Workflows** now documents a broad set of patterns that overlap with AgentRelay's core loop: Dynamic Workflows, Feature Grower, ResearchPlanAssignOps, ProjectOps, MemoryOps, a Git-backed Ledger working draft, and layered safe-output security.

OpenAI's Agents API also makes it increasingly unnecessary for AgentRelay to build its own generic coding-agent runtime: the managed Codex harness supplies sessions, orchestration, context compaction, recovery, and hosted/self-hosted execution choices.

The candidate still worth testing is a **portable, user-owned, semantically typed product-goal and evidence control plane** that connects approved goals and requirements to current acceptance evidence, implementation artifacts, releases, and operational feedback across providers, code hosts, and lifecycle systems. Factory's public positioning already approaches this claim. It is not a proven gap.

The correct next action is not to add more adapters or frameworks. It is to benchmark the strongest products and validate the user's actual pain before deciding whether AgentRelay should be a standalone product, an integration layer, a smaller open-source state component, or no separate product.

## 1. Repository inspection and prior-work gaps

At the start of this run, the repository's main branch pointed to 50d88b1733a2ee48060a64b34501d75003eb42fe (November 2 research commit). Existing research already covered the product-state model, workflow frameworks, Claude/Codex/Gemini/Jules integration, security, and evaluation.

The key gaps were:
- the competitor comparison did not yet include the depth of Factory's current Software Factory/Missions positioning;
- Shep's latest published capabilities needed a fresh review;
- GitHub Agentic Workflows had additional patterns beyond Dynamic Workflows, including iterative feature growth, project operations, research-plan-assign, cross-run memory, and a Git-backed ledger;
- OpenAI's managed Agents API/Codex harness required a clearer reuse-versus-build decision;
- the evaluation plan needed an explicit go/no-go gate tied to product-level outcomes rather than a feature checklist.

## 2. Competitive findings

### 2.1 Factory: strongest broad product-level competitor

Factory currently describes itself as an autonomous Software Factory. Its published positioning includes:
- converting enterprise intent and codebase context into validated production software;
- planning, building, reviewing, and shipping with autonomous agents;
- model routing across multiple model families;
- persistent agent computers and context;
- mission control and real-time worker progress;
- automations that plan, execute, and validate against shared context;
- integrations across code hosts, issue trackers, chat, cloud, CI, and operational tools;
- analytics and agent-readiness tracking.

Factory Missions specifically describes end-goal decomposition into dependent subtasks, resource allocation, parallel Droid execution, progress coordination, multi-repository migrations, and multi-PR feature delivery.

**Interpretation:** Factory already occupies much of the conceptual territory that AgentRelay wants to enter. Its public pages are vendor claims; they do not independently establish actual reliability, customer ROI, or complete requirement-to-runtime traceability. A hands-on trial is essential.

Sources:
- [Factory Software Factory](https://factory.com/)
- [Factory Missions](https://factory.com/product/missions)
- [Factory IDE and terminal](https://factory.com/product/ide)
- [Factory Droids](https://factory.ai/product/droids)

### 2.2 Shep v1.253: strongest local-first individual-developer baseline

Shep's current product page identifies version 1.253 and describes an MIT-licensed, local-first control center for coding agents. Published capabilities include:
- starting a new application from an idea;
- optional spec-driven requirements, research, and plan generation with approval;
- Claude Code, Codex, Copilot, Cursor, Gemini, Kimi Code, Cline, local models, and OpenAI-compatible endpoints;
- one Git worktree per feature;
- per-agent-step Git checkpoints and restore;
- agent questions with recommended answers and deadlines;
- CI watching and configurable automatic fix retries;
- circuit breakers for repeatedly failing runs;
- a web dashboard, CLI, and terminal UI;
- human-controlled merge unless explicitly allowed;
- local SQLite state.

**Interpretation:** AgentRelay cannot claim local-first execution, bring-your-own-agent, cross-provider routing, idea-to-app startup, spec approval, checkpoints, or CI recovery as unique. The depth of Shep's persistent product-wide requirement graph and runtime feedback should be investigated directly; do not infer absence from its landing page.

Source: [Shep](https://shep.bot/).

### 2.3 GitHub Agentic Workflows: a much broader overlap than previously recorded

The official GitHub Agentic Workflows documentation now describes multiple patterns that should be assessed individually.

**Dynamic Workflows**
- Program-defined stages can combine deterministic operations and agent judgment.
- Steps may run sequentially or in parallel.
- Agents can decompose a goal, pass structured results, verify one another, and pause for user input.
- Runs can be monitored, paused, cancelled, and resumed; configured limits include concurrent/total agents, active runtime, and approximate AI credits.
- Sharing a workflow definition does not share its run history or saved progress.

**Feature Grower**
- A scheduled agent reassesses a long-lived feature against its plan, current repository files, completed child issues, and advisory memory.
- It creates only the next useful implementation increment.
- An open-child gate prevents new work from being generated before the current increment is consumed.
- The pattern explicitly avoids planning the entire remaining tree before implementation feedback arrives.

**ResearchPlanAssignOps**
- Research findings become a discussion or issue.
- A planner converts them into actionable sub-issues with files, guidance, and acceptance criteria.
- Coding agents implement the issues.
- Human review and merge complete the cycle.
- The documented pattern has a human checkpoint at every phase and notes that the process is slower and depends on clean handoffs.

**ProjectOps**
- Reads GitHub Project state and generates summaries or updates fields through safe outputs.
- The documentation explicitly says maintainers remain responsible for planning and prioritization.

**MemoryOps and Git-backed Ledger**
- MemoryOps documents cache memory, repo memory in a Git branch, and comment memory.
- The Git-backed Ledger working draft specifies append-only JSONL records, content hashes, parent relationships, disposable SQLite projections, bounded history, safe-output validation, and separate trusted persistence.

**Security**
- Agentic Workflows documents isolation, permission separation, network controls, buffered safe outputs, trusted jobs, and threat detection.

Sources:
- [Dynamic Workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/dynamic-workflows)
- [Monitoring, pausing, and resuming workflows](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/use-dynamic-workflows)
- [Feature Grower](https://github.github.com/gh-aw/patterns/feature-grower/)
- [ResearchPlanAssignOps](https://github.github.com/gh-aw/patterns/research-plan-assign-ops/)
- [ProjectOps](https://github.github.com/gh-aw/patterns/project-ops/)
- [MemoryOps](https://github.github.com/gh-aw/patterns/memory-ops/)
- [Git-backed Ledger Specification](https://github.github.com/gh-aw/specs/repo-memory-ledger-specification/)
- [Security Architecture](https://github.github.com/gh-aw/introduction/architecture/)

### 2.4 OpenAI Agents API: managed execution reduces the need to build a harness

OpenAI's current Agents API describes a managed Codex harness where OpenAI handles sessions, orchestration, context compaction, and recovery while the application supplies tools and chooses an execution environment. It supports hosted and self-hosted sandbox approaches.

For self-hosted execution, the documented pattern uses codex exec-server in a laptop, container, or remote sandbox. The executor connects outbound using a separate restricted environment key. The application API key should remain outside the sandbox.

**Interpretation:** AgentRelay should own product-level state and decision-making, not recreate provider session managers, tool loops, context compaction, or generic recovery when a supported harness already supplies them.

Sources:
- [OpenAI Agents API overview](https://developers.openai.com/api/docs/guides/agents-api/overview)
- [Codex as a platform](https://developers.openai.com/blog/codex-as-a-platform)
- [Self-hosted sandboxes](https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted)

## 3. Reassessment of AgentRelay's differentiation

| Hypothesis | Evidence after this review | Current assessment |
|---|---|---|
| Persistent project continuity | Factory, Shep, Copilot Memory, GitHub MemoryOps and repo ledger | Weak alone |
| Evidence-based handoffs | GitHub workflows, Shep checkpoints/CI/PRs, Factory validation/review | Weak to moderate; product-level evidence traceability needs direct tests |
| Coding-environment adapters | Shep and Factory already advertise broad provider/tool coverage | Weak alone |
| Policy controls and approvals | Shep, GitHub safe outputs, provider harness permissions, Factory governance | Weak alone |
| Developer experience | Shep dashboard/CLI/TUI; Factory multi-surface UX; GitHub native workflows | Weak alone |
| Product goal → requirement → acceptance → commit → release → runtime feedback | Pieces exist across Factory, GitHub, CI and telemetry; no independent comparative result in this research | Candidate, not proven |
| User-owned cross-provider and cross-tool project state | Plausible; Factory is broad and GitHub is deep within its ecosystem | Candidate, not proven |

The product thesis must not be defended by renaming existing orchestration features. A semantic project ledger is useful only if it improves decisions and outcomes.

## 4. Architecture implications

### 4.1 Preserve the control-plane/execution-plane boundary

AgentRelay should own approved product goals, users, constraints, success measures, versioned requirements, assumptions, decisions, architecture, product roadmap, next-action selection, evidence-to-requirement mapping, policy, approvals, budgets, escalation, and cross-provider continuity.

Existing harnesses and workflow platforms should own provider-specific agent loops, sessions, context compaction, shell/file/tool execution, native workspace isolation, permissions, raw provider events, and generic workflow/CI scheduling where adequate.

### 4.2 Product state is not generic memory

GitHub's ledger work shows that append-only JSONL, content hashes, Git-backed persistence, and disposable query projections are available design patterns. They are not a product moat.

AgentRelay's candidate value is typed semantic relationships:
Goal → RequirementVersion → AcceptanceCheck → PlanVersion/WorkItem → AgentRun → Artifact → Evidence → Release → OperationalSignal.

A record hash proves byte integrity, not authorship or truth. Evidence is current only for the exact requirement version, commit/tree, environment, check version, and release it covers. Relevant changes can invalidate earlier evidence.

### 4.3 Reconcile external side effects

For actions such as creating a PR, merging, deploying, migrating a database, or changing an external issue:
- create a stable action ID and idempotency key;
- record the expected external version/state;
- apply only after policy authorization and required approval;
- if a request times out, query the external system before retrying;
- distinguish confirmed, failed, unknown, and needs-human-reconciliation states.

A timeout does not prove that an external effect did not happen.

## 5. Security findings

GitHub Agentic Workflows provides a useful reference for separating untrusted agent execution from trusted persistence:
- agent jobs do not receive broad write credentials by default;
- writes are buffered as structured artifacts;
- trusted jobs validate and apply permitted effects;
- persistence rechecks transactions against current canonical state;
- credentials and network access are scoped.

AgentRelay should use the same principle even if it uses another runtime. Do not allow an agent to directly rewrite canonical project state or hold broad external credentials.

The Git-backed Ledger working draft explicitly warns that hashes do not authenticate authorship or truth, and that memory payloads are untrusted content. AI threat detection is defense in depth, not a proof of safety.

The empirical study of GitHub Agentic Workflows analyzed 1,248 files from 276 repositories and found that only 9.4% of the labeled workflow files explicitly addressed prompt-injection defense. This is a specific sample, not a general prevalence estimate, but it reinforces the need to make threat boundaries explicit.

Sources:
- [GitHub security architecture](https://github.github.com/gh-aw/introduction/architecture/)
- [Git-backed Ledger](https://github.github.com/gh-aw/specs/repo-memory-ledger-specification/)
- [Empirical workflow study, 2026-09-23](https://arxiv.org/abs/2609.27263)
- [OpenAI sandbox security](https://developers.openai.com/api/docs/guides/agents-api/environments/security)
- [Temporal agent gateway idempotency notes](https://docs.temporal.io/ai/featured/agent-gateway-nexus)

## 6. Evaluation changes

The benchmark must compare AgentRelay against Factory, Shep, GitHub Agentic Workflows, Jules, and a strong single-agent workflow. Use the same project goals, acceptance criteria, models where possible, budget, permissions, and human time. Where equivalence is impossible, disclose the difference.

Measure verified requirement completion, manager next-action quality, goal-to-evidence traceability, stale-evidence detection, requirement-change impact analysis, provider substitution and recovery, duplicate/unauthorized external effects, human coordination minutes, total elapsed time and cost, and setup friction.

Add scenarios for multi-repository work, a mid-project goal change, prompt injection in repository content, long-lived feature backpressure, remote side effect after timeout, stale evidence, provider outage, ambiguous requirements, and a deployment requiring approval.

Do not use agent count, task count, generated issues, PR volume, or autonomy level as primary success metrics. The primary outcome is time and cost to a verified product outcome with less human coordination and no material quality or safety regression.

## 7. Product validation and next steps

1. Hands-on test Factory Missions and its Software Factory workflow.
2. Hands-on test Shep's idea-to-app, spec, checkpoint, CI retry, and approval paths.
3. Test GitHub Agentic Workflows Feature Grower and ResearchPlanAssignOps, including actual persistence and approval behavior.
4. Test whether the same project goal can be carried across a code host, issue tracker, CI system, and release/monitoring source.
5. Conduct 5–10 interviews based on people's most recent real AI-assisted software project. Ask about actual context reconstruction, failures, approvals, time spent, and existing workarounds—not whether they like AgentRelay.
6. Run 3–5 concierge pilots: manually maintain the proposed goal/requirement/evidence ledger while users continue using their existing coding agents.

After validation, choose among a standalone product, an integration layer, an open-source state/evidence component, or no separate product.

## 8. Open questions

- Do individual builders experience enough coordination pain to adopt another product?
- Is cross-provider switching frequent and costly enough to justify a durable manager?
- Can AgentRelay understand product outcomes beyond repository state and tests?
- What external signals (usage analytics, support tickets, incidents, customer feedback) can it safely use to replan?
- Can a user-owned ledger remain portable without becoming another manual source of truth?
- Which execution substrate offers the best first integration without creating auth or subscription-policy risk?
- What is the smallest proof that AgentRelay improves the manager's decision quality rather than simply automating more actions?
- Is the best initial customer an individual builder, a small software team, or a platform engineering group?

## 9. Sources reviewed on 2026-11-03

- [Factory Software Factory](https://factory.com/)
- [Factory Missions](https://factory.com/product/missions)
- [Factory IDE and terminal](https://factory.com/product/ide)
- [Factory Droids](https://factory.ai/product/droids)
- [Shep](https://shep.bot/)
- [GitHub Copilot Dynamic Workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/dynamic-workflows)
- [GitHub Dynamic Workflow usage and resume](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/use-dynamic-workflows)
- [GitHub Agentic Workflows Feature Grower](https://github.github.com/gh-aw/patterns/feature-grower/)
- [GitHub Agentic Workflows ResearchPlanAssignOps](https://github.github.com/gh-aw/patterns/research-plan-assign-ops/)
- [GitHub Agentic Workflows ProjectOps](https://github.github.com/gh-aw/patterns/project-ops/)
- [GitHub Agentic Workflows MemoryOps](https://github.github.com/gh-aw/patterns/memory-ops/)
- [GitHub Agentic Workflows Git-backed Ledger](https://github.github.com/gh-aw/specs/repo-memory-ledger-specification/)
- [GitHub Agentic Workflows security architecture](https://github.github.com/gh-aw/introduction/architecture/)
- [OpenAI Agents API](https://developers.openai.com/api/docs/guides/agents-api/overview)
- [OpenAI Codex as a platform](https://developers.openai.com/blog/codex-as-a-platform)
- [OpenAI self-hosted sandboxes](https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted)
- [OpenAI sandbox security](https://developers.openai.com/api/docs/guides/agents-api/environments/security)
- [Claude Agent SDK](https://code.claude.com/docs/en/agent-sdk/overview)
- [Jules Sessions API](https://jules.google/docs/api/reference/sessions)
- [Temporal agent gateway idempotency](https://docs.temporal.io/ai/featured/agent-gateway-nexus)
- [Empirical study of GitHub Agentic Workflows](https://arxiv.org/abs/2609.27263)
