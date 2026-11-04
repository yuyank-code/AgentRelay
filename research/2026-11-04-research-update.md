# AgentRelay Research Update — 2026-11-04

**Research date:** 2026-11-04  
**Repository head inspected:** `78da75accae5fef85fe8c29ca8021ff5bed19a74` (main; November 3 research batch)  
**Scope:** new competitor and execution-platform developments; adapter semantics; provider-managed approval and verification; memory integrity; long-horizon evaluation; cumulative product and architecture implications.  
**Evidence standard:** official documentation establishes documented capabilities and limitations, not independent product quality. RoadmapBench is an academic benchmark with a defined task suite and harness, not a direct evaluation of AgentRelay's manager behavior. No competitor accounts were operated and no user interviews were conducted in this run.

## Executive conclusion

The competitive bar moved again. Two provider ecosystems now expose much more of the development-management substrate than a simple model API:

1. **Cursor Cloud Agents and Automations** provide a public-beta API for durable agents and individual runs, model selection, steering/queued follow-up prompts, SSE run events, cloud or self-hosted workers, multi-repository environments, event/scheduled automations, and persistent automation memory.
2. **Claude Managed Agents** (beta) provide stateful sessions, cloud or self-hosted execution, event streams, persistent server-side history, subagents, agent-generated dynamic workflows, budgets, scheduled deployments, and rubric-based outcome evaluation with iterative revision.

OpenAI's Codex platform strategy explicitly encourages developers to embed its reusable harness inside domain-specific products while the host application owns the interface, context, tools, and operational boundaries.

These developments make the original feature-level differentiation even less defensible. AgentRelay cannot win merely by launching agents, planning work, remembering context, using multiple agents, scheduling runs, applying approval gates, or grading an artifact against a rubric.

The candidate still worth testing is a **provider-neutral product-goal control plane** that preserves approved product intent and traces it to current, reproducible acceptance evidence across different execution systems, repositories, releases, and operational feedback. Even that is only a hypothesis: Factory, GitHub, Cursor, Shep, and provider platforms already cover important pieces. The next step remains hands-on comparison and user validation, not a broad implementation.

## 1. Repository inspection and gaps found

At the start of this run, `main` pointed to `78da75accae5fef85fe8c29ca8021ff5bed19a74`. The repository already contained the product vision, product-state ledger, architecture, integration feasibility, competitor analysis, security model, evaluation plan, and dated research through November 3.

Gaps identified:
- Cursor Cloud Agents API v1 and Automations were not yet included in the competitor and adapter assessments.
- Claude Managed Agents' outcome rubric, dynamic workflow, session, sandbox, and memory capabilities were not represented.
- The adapter model did not explicitly distinguish provider run completion from AgentRelay task acceptance.
- The security model needed to address cross-run memory poisoning and the limits of provider-native automatic reviewers.
- The benchmark plan did not yet incorporate RoadmapBench's multi-target partial-completion metrics and the new provider-specific failure semantics.
- `research/README.md` had a stale latest-update date (November 2) despite the November 3 report existing; the main README did not link the product-state model.

## 2. Cursor Cloud Agents and Automations

### Documented capabilities

Cursor's Cloud Agents API v1 is documented as **public beta**. Its create-agent endpoint creates a durable agent and immediately queues an initial run. Subsequent prompts create runs that can be steered into a running turn or queued. The API exposes run listing, run retrieval, and server-sent event streaming. Agents can target Cursor-hosted cloud environments or self-hosted pools/machines, and can work with connected GitHub, GitLab, Bitbucket Cloud, and Azure DevOps repositories. The create request can select a model, configure MCP servers and custom subagents, and choose plan or agent mode.

The v1 API page says webhooks are coming soon; the legacy v0 API still documents webhook support. The v1 SSE stream is scoped to one run and does not replay prior runs. The run response's Git branch/PR snapshot is per-agent rather than per-run, so an adapter must attribute changes using run IDs, latest-run information, and event streams rather than assuming that the agent-level Git snapshot belongs to the requested run.

Cursor Automations run cloud agents on schedules or in response to GitHub, GitLab, Slack, webhooks, Linear, Sentry, PagerDuty, and other events. They can use one or multiple repositories, choose a model, call MCP tools, open pull requests, and retain named memory entries across runs. Billing is based on cloud-agent usage. Cursor explicitly warns that untrusted inputs can cause misleading or malicious persistent memories.

### Competitive implication

Cursor is now a strong direct baseline for a developer who wants agents to continue working without a local machine or manual prompt handoffs. It overlaps with scheduling, event-driven work, multi-repository context, memory, model choice, tool integrations, self-hosted execution, and PR workflows.

Public docs do not establish that Cursor offers a portable product-goal ledger across arbitrary coding-agent runtimes or that its automations own a complete vendor-neutral product lifecycle. Do not infer that those capabilities are absent; test them hands-on. Cursor is also a plausible execution adapter, not just a competitor.

### Adapter implications

- Treat an agent and its runs as separate resources. Persist both `agent_id` and `run_id`.
- Model `steer` and `queued` as different delivery outcomes.
- Support SSE ingestion plus polling/reconciliation because the v1 event stream does not replay old runs and v1 webhooks are not yet documented as available.
- Preserve provider event IDs and run IDs; deduplicate events before changing project state.
- Do not attach an agent-level Git snapshot to a specific run without verifying the branch/PR and commit.
- Record the chosen model, model parameters, environment type, repository starting ref, billing identity, and cost/usage returned by the provider.
- Mark the adapter's beta and webhook limitations in its capability profile.

## 3. Anthropic Claude Managed Agents

### Documented capabilities

Claude Managed Agents is a beta, pre-built agent harness for long-running and asynchronous work. The documented model is Agent + Environment + Session + Events. Environments can use Anthropic-managed cloud sandboxes or self-hosted workers. Sessions retain server-side history and state; event streams and session APIs support progress, steering, and interruption. The platform includes tool configuration, MCP, memory stores, budgets, webhooks, scheduled deployments, and multi-agent orchestration.

The multi-agent feature supports persistent subagent threads, agent-generated dynamic workflows that run agents in phases, fan-out and programmatic result passing, advisor-model consultation, and session-level budgets.

A particularly important semantic detail: dynamic workflow code is written by the agent and starts in the same turn; the documentation says there is no separate approval step before a run starts. A workflow result of `completed` means the workflow program finished, **not** that every agent thread succeeded or that the work passed. AgentRelay must inspect thread-level events and artifacts.

Claude Managed Agents also has a rubric-based **outcome** feature. A session receives an intended outcome and a gradeable rubric; a separate context-window grader evaluates the artifact, returns criterion-level feedback, and the agent can iterate. The documented default is three iterations, with a maximum of 20. This directly overlaps with acceptance contracts and self-correction for bounded artifacts, although the public docs do not establish a portable product-wide requirement ledger across providers.

Self-hosted sandboxes keep tool execution and files on infrastructure controlled by the customer, but tool inputs and outputs still flow to Managed Agents on Anthropic's side. Managed Agents and its subfeatures are not currently eligible for Zero Data Retention or HIPAA BAA coverage. This must be considered before handling sensitive repositories or data.

### Competitive implication

This is a significant provider-native alternative to building a general agent runtime or manager workflow from scratch. A Claude-only product could build a substantial amount of planning, delegation, outcome grading, session continuity, and background execution on top of Managed Agents.

It is not provider-neutral: the model and agent runtime are Claude-specific, and the features are beta. The opportunity for AgentRelay would have to be above this substrate—approved product intent, cross-provider continuity, lifecycle evidence, independent policy, and integration with external systems—not a duplicate of its agent loop or workflow engine.

### Claude Agent SDK hosting detail

The local Agent SDK starts a Claude CLI subprocess per session. Session transcripts, memory files, and working-directory artifacts live on local disk by default and do not survive container restarts, scale-down, or movement to another node. Transcript persistence requires a SessionStore adapter; memory and workspace artifacts need their own storage strategy. This makes a provider session ID alone insufficient for AgentRelay's durability guarantees.

The official SDK overview also says third-party products may not offer Claude.ai login or subscription rate limits without prior approval; API-key authentication is the documented default for product integrations. Do not design a commercial AgentRelay authentication flow around an assumed right to reuse a consumer subscription.

## 4. OpenAI Codex as an embeddable harness and provider review

OpenAI's August 19, 2026 "Codex as a platform" post describes the open-source Codex harness as reusable infrastructure. It recommends `codex exec` for bounded scripts/CI, the Codex SDK for programmatic tasks, and app-server for persistent conversations, streaming, interruption, custom tools, and approval handling. The post's central architecture message is that the host product should own its domain interface, context, tools, and operational boundaries while reusing the agent harness.

The Agents API's automatic review feature uses a provider-managed reviewer model to approve or deny supported permission requests. Review tokens are billed in addition to agent usage. It does not override environment network restrictions. Setting review to `none` does not route actions to the application for manual approval; most actions are governed by the environment's own permissions. Therefore, provider-native review is useful defense-in-depth but is not a substitute for AgentRelay's own policy and authorization service.

## 5. Cross-provider architectural conclusions

### 5.1 Separate provider run state from product acceptance

Every adapter should report a provider-native lifecycle status, but AgentRelay should maintain a separate acceptance state.

Suggested separation:
- `provider_run_status`: queued, starting, running, waiting, completed/finished, failed/error, cancelled, expired, unknown.
- `delivery_status`: accepted, steered, queued, rejected, unknown.
- `task_acceptance_status`: unverified, evidence_pending, passed, failed, inconclusive, stale, blocked, human_accepted.
- `requirement_status`: proposed, clarified, accepted, in_progress, implemented_unverified, verified, regressed, waived_by_owner.

A provider run ending successfully is a transport/execution fact, not proof that the assigned work satisfies its acceptance criteria. Anthropic's own workflow docs explicitly distinguish a completed workflow from the success of its child threads. Cursor's terminal run status exposes a final result and Git state, not product acceptance.

### 5.2 Strengthen the adapter capability contract

For each adapter, record whether it supports:
- create/start, follow-up/steer, queue, pause, interrupt/cancel, resume;
- event streaming, replay, webhook delivery, polling, and event sequence/cursor;
- run-scoped versus agent/session-scoped artifacts and Git references;
- model selection and supported parameters;
- workspace type, filesystem scope, network policy, and secret injection;
- permission callbacks, native approval modes, and whether approvals reach the host application;
- usage/cost reporting, budget controls, billing identity, and retention;
- provider version, API maturity, known limitations, and auth/terms constraints.

Do not normalize away missing capabilities. A capability profile should state `supported`, `unsupported`, `beta`, or `unknown`, with evidence and last-checked date.

### 5.3 Event ingestion and reconciliation

Persist the provider's raw event reference, adapter version, project, work item, agent/session/run IDs, source timestamp, received timestamp, provider sequence/cursor when available, and raw-event reference. Deduplicate by provider event ID or a deterministic fallback key. Detect gaps after stream disconnects and reconcile by fetching the run/session history. Treat provider event streams as observations, not the canonical product ledger. Reconcile external effects against the external system before retrying an action.

### 5.4 Evidence freshness

Every verification result should bind to the requirement ID/version, exact commit/tree SHA or immutable artifact digest, environment image/configuration, dependency lock state, check identifier/version, command and parameters, result, evaluator, timestamp, logs, and limitations. Mark evidence stale when a linked requirement, source commit/artifact, environment, dependency lock, or check definition changes materially. A new commit should not inherit a prior green result by default.

### 5.5 Memory integrity

Separate untrusted observations and agent suggestions from approved product state. An issue, PR comment, web page, tool result, or agent-generated memory must not silently change the goal, requirement, policy, credential scope, or trusted architecture decision. Promotion into canonical project memory should require provenance, validation, and human approval when scope or security changes.

## 6. Evaluation updates

RoadmapBench (May 15, 2026; revised May 19) evaluates 115 real multi-target version-upgrade tasks across 17 repositories and five languages. The paper reports a median task affecting about 3,700 lines across 51 files. In its OpenHands setup, the top reported model, Claude Opus 4.7, resolved 39.1%; the paper's Completion Score was 0.692. The authors emphasize that partial completion is common and that integration boundaries remain difficult.

This is an executor benchmark, not a direct test of AgentRelay's product manager. Use it as evidence that multi-target implementation and integration remain hard, and borrow the distinction between full resolution and weighted partial completion.

Add these evaluation cases:
1. Cursor run's SSE stream disconnects; reconcile status and artifact references without duplicating work.
2. Two runs occur on one Cursor agent; verify Git branches/PRs are attributed to the correct run.
3. Claude dynamic workflow returns `completed` while one child thread failed; do not accept the task.
4. An outcome grader returns `satisfied`, but deterministic integration tests fail; keep acceptance failed/inconclusive.
5. A memory entry derived from malicious issue text attempts to change project scope or expose secrets; block promotion.
6. Provider-native automatic review approves an action that AgentRelay policy forbids; the independent policy boundary must still deny it.
7. A later commit changes code after a previously passing check; mark evidence stale.

Report both full requirement resolution and weighted partial completion. Continue measuring verified goal completion, requirement-to-evidence coverage, next-action quality, stale-evidence detection, recovery correctness, provider portability, human coordination minutes, total cost, latency, and safety violations. Never optimize for raw agent count, task count, run count, or PR count.

## 7. Product and roadmap decision

The broad AI development-manager vision remains the north star, but the differentiation claim is still unproven. Cursor and Claude Managed Agents make it especially risky to build a new generic agent runner, scheduler, memory store, subagent system, or rubric grader.

Recommended validation order:
1. Run hands-on trials of Cursor Cloud Agents/Automations and Claude Managed Agents against Factory, Shep, GitHub Agentic Workflows, Jules, and a strong single-agent baseline.
2. Test the same project goal across at least two genuinely different execution substrates, including a mid-project provider switch.
3. Measure whether the product-goal/evidence ledger reduces human coordination and correctly detects requirement gaps and stale evidence.
4. Compare the value of a standalone manager with an integration that adds a portable state/evidence layer to an existing tool.
5. Only then select the first runtime and build the vertical slice.

No production implementation is recommended by this research batch. No competitor account trials or customer interviews were performed; all competitive findings are desk research.

## 8. Sources reviewed (accessed 2026-11-04)

### Cursor
- Cloud Agents API v1: https://cursor.com/docs/cloud-agent/api/endpoints
- Cloud Agent Automations: https://cursor.com/docs/cloud-agent/automations
- Cloud Agent security/network: https://cursor.com/docs/cloud-agent/security-network
- Self-hosted machines: https://cursor.com/docs/cloud-agent/self-hosted
- Cloud Agents overview: https://cursor.com/docs/cloud-agent

### Anthropic
- Claude Managed Agents overview: https://platform.claude.com/docs/en/managed-agents/overview
- Multi-agent orchestration: https://platform.claude.com/docs/en/managed-agents/multiagent-orchestration
- Workflow runs: https://platform.claude.com/docs/en/managed-agents/workflow-runs
- Define outcomes: https://platform.claude.com/docs/en/managed-agents/define-outcomes
- Secure deployment: https://platform.claude.com/docs/en/managed-agents/secure-deployment
- Self-hosted sandboxes: https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes
- Claude Agent SDK overview: https://code.claude.com/docs/en/agent-sdk/overview
- Claude Agent SDK hosting: https://code.claude.com/docs/en/agent-sdk/hosting
- Claude Agent SDK permissions: https://code.claude.com/docs/en/agent-sdk/permissions

### OpenAI
- Codex as a platform, August 19, 2026: https://developers.openai.com/blog/codex-as-a-platform
- Agents API automatic review: https://developers.openai.com/api/docs/guides/agents-api/sessions/approvals
- Agents API architecture: https://developers.openai.com/api/docs/guides/agents-api/architecture

### GitHub and evaluation
- GitHub Agentic Workflows: https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows
- GitHub Agentic Workflows threat detection: https://github.github.com/gh-aw/reference/threat-detection/
- RoadmapBench, arXiv:2605.15846: https://arxiv.org/abs/2605.15846
- Temporal workflow determinism and replay: https://docs.temporal.io/workflows

## Next research questions

- Can Cursor Automations or Claude Managed Agents already deliver the desired user experience with only a thin product-state service?
- Which provider offers the most reliable event history, artifact retrieval, cancellation, and cost reporting for a first adapter?
- Can a portable goal/requirement/evidence model survive provider switches without degrading acceptance quality?
- How should AgentRelay score partial completion and uncertainty without turning an LLM grader into the source of truth?
- What is the smallest cross-provider scenario where a user measurably saves coordination time?
- Would users prefer a standalone manager, a repository-native open state/evidence spec, or a plugin/extension for an existing orchestrator?
