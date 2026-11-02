# AgentRelay Research Update — 2026-11-02

**Repository:** [yuyank-code/AgentRelay](https://github.com/yuyank-code/AgentRelay)  
**Research date:** 2026-11-02  
**Scope:** Competitor workflow depth, user-workflow signals, product-state architecture, provenance, durability, observability, security and validation.  
**Evidence standard:** Official documentation/specifications first; community reports are explicitly anecdotal. No hands-on competitor trial or customer interview was completed in this run.

## Executive summary

Today's review materially raises the competitive bar. GitHub Copilot dynamic workflows document multi-stage and parallel agent workflows, structured result passing, subagent verification, human checkpoints, pause/resume and budgets. GitHub Agentic Workflows can run multiple coding-agent engines under repository guardrails. Copilot Memory spans several Copilot surfaces. Google Jules provides plan-before-code, approval, task sessions, activities, follow-up, retries and an alpha REST API.

Therefore, AgentRelay cannot credibly differentiate on agent routing, orchestration, memory, approval gates, run monitoring or retry loops alone.

The strongest remaining hypothesis is a **provider-neutral product-state and evidence layer** that preserves the user's approved outcome and connects requirements, decisions, plans, code changes, verification evidence, releases and operational feedback across tools and providers. This remains unproven: competitors may already cover more of it than their public docs reveal, and the value must be demonstrated in hands-on trials and user interviews.

A recent community discussion also provides a concrete DIY workflow close to the vision: a non-developer builder uses a master design document, Claude to manage tickets and review, scheduled Codex work to implement, and human testing/bug reports to drive the next cycle. This is a useful workflow lead, not market validation.

## 1. Repository baseline and gap addressed

At the start of this run, `main` pointed to `7fd4a0be9deae1ec1a68284d4feedcd4faaf87c0`. Existing docs already framed AgentRelay as a goal-oriented AI development manager and distinguished it from provider coding harnesses.

The main architecture gap was the meaning of "persistent project continuity." Existing docs did not fully separate product/domain truth, workflow execution state, external side effects, agent context/memory, and telemetry/audit evidence. This update adds a logical product-state/evidence model and ADR-0004.

## 2. Competitor refresh

### 2.1 GitHub Copilot dynamic workflows

GitHub documents dynamic workflows as programs combining deterministic code and agents. They can run sequentially or in parallel, split a goal into tasks, pass structured results, have subagents verify findings, request user input, and pause at checkpoints. Runs can be monitored, limited by active/total agents, duration and approximate AI credits, paused and resumed.

Important boundary: sharing a workflow extension shares its definition, not the original run history or saved progress. A paused run can reuse saved results from completed steps when resumed, but this is not equivalent to a project-level history spanning separate milestones, providers and releases.

**What is table stakes:** multi-step orchestration, conditional/parallel work, agent result passing, subagent review, progress visibility, checkpoints, pause/resume and spend limits.

Sources: [Dynamic workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/dynamic-workflows), [Using dynamic workflows](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/use-dynamic-workflows).

### 2.2 GitHub Agentic Workflows

GitHub Agentic Workflows define repository automations in Markdown and run them through GitHub Actions. Current docs list Copilot, Claude, Codex and Gemini engines. The documented security design includes read-only repository permissions by default, declared safe outputs, secrets held in isolated downstream jobs, firewalled execution and threat detection. Cost combines Actions minutes and model inference; AI Credit estimates are not exact provider invoices.

This is a strong alternative for recurring repository chores and event-driven automation. AgentRelay should reuse these workflows where they solve the job rather than build another repository scheduler.

Source: [About GitHub Agentic Workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows).

### 2.3 GitHub Copilot Memory

Copilot Memory stores repository-level facts and user-level preferences and can be used across Copilot app, CLI, cloud agent, code review, Slack and Teams. It cites repository facts to code and can re-check citations before use. However, repository facts are scoped to a single repository, memory use is a model judgment rather than a guaranteed rule, and the documented default expiry is 90 days without reinforcement.

This means "persistent memory" is not a unique differentiator. AgentRelay's possible value is explicit, user-owned, exportable and provider-neutral product state—not another vector store or summary file.

Source: [About GitHub Copilot Memory](https://docs.github.com/en/copilot/concepts/prompting-and-context/copilot-memory).

### 2.4 Google Jules

Jules documents plan-before-code, user review/approval of the plan, mid-task feedback, plan revision, activity feed, diff preview, pausing, and automatic retry of some transient failures.

The Jules REST API is currently alpha. It models Source, Session and Activity; supports creating a repository-backed session, polling sessions/activities, sending follow-up messages and approving plans. API-created sessions automatically approve the plan unless `requirePlanApproval=true` is set. An optional `AUTO_CREATE_PR` mode can create a pull request.

Jules is a credible future adapter candidate because it exposes an official API, but alpha status and plan-approval defaults require explicit version and policy handling. The documented session model is task-centric; the reviewed docs do not establish a portable product goal/evidence graph, but that is not proof the product lacks one.

Sources: [Jules getting started](https://jules.google/docs/), [Running tasks](https://jules.google/docs/running-tasks/), [Reviewing plans](https://jules.google/docs/review-plan/), [Errors and failures](https://jules.google/docs/errors/), [REST API](https://jules.google/docs/api/reference/).

### 2.5 Competitive conclusion

| Capability | Assessment |
|---|---|
| Agent routing / model choice | Commoditized |
| Multi-agent orchestration / parallelism | Commoditized |
| Task plan and plan approval | Commoditized |
| Persistent memory | Present in major ecosystems |
| Run history, checkpoints, retries and pause/resume | Present in multiple products |
| Multi-engine repository automation | Present in GitHub |
| Cross-provider product-state ledger with versioned requirements and evidence freshness | Potential wedge; not validated |
| Requirement-to-release-to-runtime traceability | Potential wedge; hands-on competitor research needed |
| Independent project-level outcome evaluation | Potential research/product wedge; requires published benchmark evidence |

Do not claim competitors lack a feature because public docs do not mention it. Compare the actual products on the same workflow.

## 3. User-workflow evidence

A recent r/ClaudeCode discussion (reviewed 2026-11-02) includes a commenter who describes themselves as a non-developer building a complex business website. Their workflow uses a master design document, Claude to refine and oversee a ticket system, scheduled Codex work to implement tickets, Claude to review, and human testing/bug reports that return to the queue.

Other comments describe feeding meeting/team communications into a wiki and issue tracker, using research/refinement/review loops, deterministic gates, and a meta-agent that escalates when human attention is needed. Another comment recommends a simpler orchestrator that writes a small task spec, assigns one writer per worktree, and uses an independent reviewer with deterministic checks.

This is close to the AgentRelay target experience. It also exposes a risk: users can already approximate this workflow with existing tools and scripts. AgentRelay must show that its integrated state/evidence model saves time or improves reliability enough to justify a new product.

**Evidence limitation:** this is a self-selected Reddit discussion, not a representative sample, customer interview or willingness-to-pay signal.

Source: [How do you guys orchestrate multiple coding agents for software engineering?](https://www.reddit.com/r/ClaudeCode/comments/1x3a7ni/how_do_you_guys_orchestrate_multiple_coding/).

## 4. Architecture research: project state is not agent memory

Temporal's October 2026 material distinguishes:
- **Context:** information in front of the model for its current decision.
- **Long-term memory:** facts, prior cases and procedures retrieved from a store.
- **Execution state:** which workflow steps have completed and what comes next.
- **Effect state:** whether an external action actually happened, as recorded by the system that performed it.

AgentRelay should add first-class **product/domain state**: the approved goal, requirements, constraints, decisions, plan versions, evidence, releases and operational feedback.

This yields five layers:
1. Product/domain state — AgentRelay's canonical record.
2. Execution state — workflow runtime and event history.
3. External effect state — GitHub, CI, deployment, issue tracker and other systems remain authoritative.
4. Agent context/memory — compiled from canonical records for each task.
5. Observability — traces and metrics describe behavior, not product correctness.

Source: [Temporal: Agent memory and state](https://docs.temporal.io/ai/featured/memory-state-stripe).

### 4.1 Provenance model

W3C PROV-O provides a useful vocabulary:
- Entity: requirement version, code diff, test report, release artifact or decision;
- Activity: research, implementation, test, review, deployment or replanning;
- Agent: person, organization, coding agent or tool;
- relationships: used, generated, derived from, informed by, associated with and acted on behalf of.

AgentRelay need not use RDF or an ontology database. It can use relational tables and explicit links while preserving the semantics.

Source: [W3C PROV-O Recommendation](https://www.w3.org/TR/prov-o/).

### 4.2 Evidence freshness

Requirement status and evidence status must be separate. A task may be complete while a requirement remains unverified. Evidence becomes stale when the requirement version, code commit, environment or relevant test/check changes.

A verification record should identify the exact requirement version, repository commit/tree, environment, command/check, result, evaluator and timestamp. Model review is advisory; deterministic tests and independent acceptance checks carry more weight.

### 4.3 Durable workflow runtime is not product intelligence

Temporal can durably record workflow event history and reconstruct execution after a worker/process failure. It does not decide whether a product requirement is satisfied, nor does it own external system truth. Side effects still require idempotency and reconciliation because an operation may have succeeded externally before a worker loses the response.

Recommendation: keep Temporal as a later option for workflows needing multi-day durability, external signals, human waits and crash recovery. For a local vertical slice, an explicit state machine plus relational state/event store may be simpler. Do not combine LangGraph and Temporal without a clear separation between adaptive reasoning and durable execution.

Sources: [Temporal Workflows](https://docs.temporal.io/workflows), [Temporal design patterns](https://docs.temporal.io/design-patterns), [Temporal agent memory and state](https://docs.temporal.io/ai/featured/memory-state-stripe).

## 5. Observability and privacy

OpenTelemetry's current GenAI conventions include workflow invocation, agent invocation, tool execution and evaluation-result concepts. The GenAI conventions are marked **Development** and can change.

The conventions warn that prompts, completions, tool arguments/results, memory queries and memory records may contain sensitive or personal data. They recommend not capturing inputs/outputs by default and using external storage with separate access controls where content retention is required.

AgentRelay should use metadata-only traces by default; keep raw prompts, diffs, logs and tool results in separately permissioned artifact storage; put references/digests in traces rather than full content; require explicit scoped opt-in for content capture; and pin semantic-convention versions.

Sources: [GenAI agent spans](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-agent-spans.md), [GenAI spans and content capture](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md), [GenAI events](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-events.md).

## 6. Product and architecture implications

1. Treat the product goal and its linked evidence as the durable unit of work—not a conversation or agent session.
2. Keep product state, workflow execution, external effects, memory/context and telemetry separate.
3. Make requirements, plan versions, decisions and evidence explicitly versioned.
4. Reconcile PR/merge/deployment state with the external system before reporting completion or retrying a side effect.
5. Compile task-specific context from canonical records; do not dump full histories into every prompt.
6. Add a Jules adapter only after validating API stability, approval behavior and billing.
7. Use GitHub Agentic Workflows for suitable repo automations rather than cloning their scheduler/security controls.
8. Avoid adding more agents before a single-agent manager loop demonstrates requirement-level progress and evidence freshness.

## 7. Validation plan

- Interview 5–10 solo builders/developers who have used coding agents on real projects. Ask them to reconstruct their last project, manual coordination, lost context, failure recovery, trust boundaries, existing tools and willingness to adopt.
- Run a concierge pilot with 3–5 real projects using existing agents and a manual goal/requirement/evidence ledger. Measure prompt/context reconstruction time and trust in manager-selected next actions.
- Compare GitHub dynamic workflows, GitHub Agentic Workflows, Jules, Shep and Factory on the same project.
- Benchmark an ambiguous goal, requirement clarification, five dependent implementation tasks, mid-project change, false task-success signal, stale evidence, provider interruption, external action timeout, release candidate and post-release defect.
- Measure time/cost to verified product outcome and human coordination, not task/PR count.

## 8. Defensibility assessment

The feature bundle is not a moat. Potential defensibility could emerge from a portable goal/requirement/evidence model, deep integrations into repository/issue/CI/preview/deployment/runtime systems, a transparent benchmark showing better long-horizon outcomes, privacy-respecting workflow outcome data, and trusted permissions/export/local execution.

These are hypotheses. The next proof is user research and hands-on competitor trials, not another broad framework integration.

## 9. Files changed in this batch

Added `docs/product-state-model.md`, `decisions/ADR-0004-product-state-ledger.md`, and this dated report. Updated competitor, integration, architecture, security, evaluation, roadmap, research index and README documents. All changes from this run are published together in one Git commit. No application code was implemented.
