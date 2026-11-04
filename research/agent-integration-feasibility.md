# Coding-Agent Integration Feasibility

**Reviewed:** 2026-11-01  
**Status:** Desk research against current official documentation. Validate exact versions, auth, terms, and runtime behavior before implementation.

## Executive conclusion

AgentRelay has several viable ways to invoke existing coding agents. The important architecture decision is to reuse their supported agent harnesses rather than rebuilding a generic model/tool loop.

AgentRelay should own the product goal, requirements, plan, evidence, policy, and lifecycle. Provider-native SDKs, CLIs, app servers, hosted agents, and OpenHands can execute bounded work. Each adapter must expose its real capabilities and authentication constraints.

## Provider and runtime scan

| Agent/runtime | Documented integration surface | Useful capabilities | Important boundaries |
|---|---|---|---|
| Claude Code / Claude Agent SDK | Official Python and TypeScript Agent SDK; CLI subprocess using non-interactive mode and structured output | Reuses the Claude Code agent loop, built-in tools, sessions, resume/fork, hooks, subagents, MCP, permissions, structured output, usage tracking, and OpenTelemetry. | Official SDK documentation says third-party developers may not offer Claude.ai login or subscription rate limits in their products unless previously approved. A current Help Center article also describes subscription-backed third-party app usage; see the nuanced analysis below. |
| OpenAI Codex | Codex CLI and codex exec; official SDK; documented Codex app-server protocol | App-server can create threads, start turns, stream events, interrupt work, expose tools, and handle approval requests. CLI is suitable for bounded jobs; SDK/app-server suit embedded product integrations. | App-server is a local Codex process and Codex-specific. Keep the AgentRelay domain model provider-neutral and test compatibility against pinned versions. |
| Gemini CLI | Headless CLI with JSON and JSONL output | Structured results, tool-use events, usage/latency statistics, and typed exit codes. | It is a coding-agent CLI; pin versions and test session, cancellation, and sandbox behavior. |
| Gemini API CLI | Stateless shell client for the Gemini Interactions API | Calls models and managed agents, supports background work, structured output, explicit chaining via interaction IDs, and typed errors. | It is an API client, not an interactive coding-agent runtime. Requires API-key authentication and separate API billing. |
| GitHub Copilot cloud agent and partner agents | GitHub-hosted issue, prompt, and PR workflows; third-party Claude and Codex agents are documented in public preview | Asynchronous execution, draft PRs, review-comment iteration, repository context, GitHub policy controls, and usage billing. | GitHub-centric and account/policy/billing-dependent; not a general local process adapter. |
| GitHub Agentic Workflows | Markdown-defined workflows compiled to GitHub Actions; supports Copilot, Claude, Codex, and Gemini | Natural-language repository automation, scheduled/event triggers, guarded outputs, isolated execution, usage estimates, and run auditing. | Repository automation rather than a general product-goal manager. Third-party engines require their own API credentials in the documented Actions setup. |
| OpenHands Software Agent SDK / Agent Server | Python, TypeScript, REST, and Agent Server | Local or ephemeral Docker/Kubernetes workspaces; agent conversations, events, tools, and execution APIs. | Strong execution-runtime candidate, but AgentRelay would still own product planning, goal state, policy, and goal-level verification. |
| OpenHands Automation Service | Separate service for cron/webhook scheduling, dispatch, sandbox lifecycle, and run history | Could provide or inform scheduled and event-driven agent execution. | The service is documented as beta; its scheduler/dispatch layer is separate from the SDK/Agent Server execution layer. |
| A2A-compatible remote agents | A2A Agent Cards, tasks, messages, artifacts, polling, streaming, and push notifications | Useful for independently hosted remote agents. | A local CLI is not automatically an A2A server. Verify actual support for every endpoint. |

## Anthropic authentication and subscription finding — important correction

Two current official Anthropic pages need to be read together:

1. The [Claude Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview), accessed 2026-11-01, says: unless previously approved, Anthropic does not allow third-party developers to offer Claude.ai login or subscription rate limits for their products, including agents built on the SDK. It directs developers to API-key authentication and says SDK use is governed by Anthropic's Commercial Terms.
2. The [Help Center article on using the Agent SDK with a Claude plan](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan), updated after its 2026-10-07 and 2026-06-15 notices, says that when signed in with a Claude plan, the Agent SDK, claude -p, and third-party app usage still draw from subscription limits. It separately says Max, Ultra, and Team plans include monthly API credits when using an API key from a linked Console organization. Pro is not listed among those API-credit plans.

These pages create a material scope/permission ambiguity for a product such as AgentRelay. The Help Center describes subscription-backed usage, while the SDK overview restricts third-party products from offering Claude.ai login or subscription rate limits without prior approval.

**Do not turn this into a blanket claim that every third-party integration is permitted.** For a local, user-operated setup, the Help Center suggests that signed-in plan usage may consume the user's own subscription limits. For a distributed product that offers end users Claude.ai login or subscription-backed capacity, obtain written clarification/approval from Anthropic before shipping. API-key use is a separate billing path; do not assume a Claude Pro plan includes monthly API credits.

Product implications:
- Keep authentication mode explicit in the adapter profile: user-operated plan sign-in, provider API key, enterprise/managed credential, or unsupported.
- Never collect or proxy a user's browser cookies or password.
- Do not advertise that AgentRelay includes Claude Pro usage or can pool/resell subscription limits.
- Before commercial distribution, get written confirmation for the exact local/hosted integration and account model.
- Keep provider authentication behind a replaceable adapter so a policy change does not force a redesign.

## Integration architecture guidance

### Reuse the agent harness; own the product manager

The [OpenAI Codex platform article, 2026-08-19](https://developers.openai.com/blog/codex-as-a-platform) makes the distinction explicit: the harness manages the agent loop, context, tools, streaming, sandbox/approval behavior, and continuation across turns; the host application owns the domain workflow, business context, interface, and operational boundaries.

The same division is useful for AgentRelay:
- AgentRelay: product goal, requirements, assumptions, decisions, roadmap, dependency graph, policy, approvals, evidence ledger, release state, and progress view.
- Provider harness/runtime: model loop, tool execution, context compaction, provider-specific session handling, streaming, and native permission callbacks.
- Adapter: maps provider-specific lifecycle events and capabilities into AgentRelay's internal contract without pretending unsupported features exist.

### Suggested adapter capability profile

Each adapter should publish:
- provider, product, runtime, adapter version, and supported version range;
- authentication modes, credential owner, billing route, and terms link;
- local, hosted, or remote execution model;
- repository/workspace binding and isolation model;
- file, shell, browser, network, and MCP capabilities;
- structured output schema and event stream;
- start, follow-up, resume, fork, interrupt, cancel, and timeout support;
- permission hooks and approval callbacks;
- usage, token, cost, latency, and quota reporting;
- supported operating systems, prerequisites, and known limitations.

Normalize only the outer lifecycle. Preserve raw provider events and raw artifacts so that provider-specific detail is not lost.

## Recommended integration order

1. Prove the manager loop with one supported runtime and one repository.
2. Compare a provider-native integration with OpenHands Agent Server; choose based on isolation, observability, reliability, and operational burden.
3. Add a second provider only after the goal/evidence workflow is useful with one.
4. Use GitHub Agentic Workflows for repository automation where its scope fits; do not recreate its security and scheduling features without a reason.
5. Add A2A for remote agents only when a real user workflow requires it.

## Adapter conformance tests

Every adapter should pass tests for:
- correct repository and workspace binding;
- structured start, progress, and completion/error events;
- non-zero exits, provider errors, rate limits, and malformed output;
- timeout, interruption, cancellation, and resume where claimed;
- permission denial and approval callback behavior;
- output capture, secret redaction, and credential isolation;
- cost/usage reporting or explicit unsupported status;
- crash recovery without duplicate external side effects;
- pinned-version compatibility and a documented fallback.

## Sources reviewed on 2026-11-01

- [Claude Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview)
- [Use the Claude Agent SDK with your Claude plan](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)
- [Codex as a platform: build on the open agent harness, 2026-08-19](https://developers.openai.com/blog/codex-as-a-platform)
- [Gemini CLI headless mode](https://geminicli.com/docs/cli/headless/)
- [Gemini API CLI](https://ai.google.dev/gemini-api/docs/cli)
- [GitHub third-party coding agents](https://docs.github.com/en/copilot/concepts/agent-capabilities/about-agents/about-third-party-coding-agents)
- [GitHub Agentic Workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows)
- [Develop agentic workflows in GitHub Actions](https://docs.github.com/en/actions/tutorials/develop-agentic-workflows-in-github-actions)
- [OpenHands Software Agent SDK](https://github.com/OpenHands/software-agent-sdk)
- [OpenHands Automation Service](https://github.com/OpenHands/automation)
- [A2A core concepts](https://a2a-protocol.org/latest/topics/key-concepts/)


## 2026-11-02 integration refresh: Jules and GitHub workflows

### Google Jules REST API

The official Jules REST API quickstart currently labels the API alpha/experimental. Its resource model is:
- **Source:** connected input source such as a GitHub repository; the Jules GitHub app must first be installed.
- **Session:** continuous unit of work with a prompt and source context.
- **Activity:** item within a session, including plan generation, user messages and agent progress.

The API quickstart documents listing sources, creating a repository/branch-backed session, polling sessions and activities, sending follow-up messages, and approving a plan. An optional `AUTO_CREATE_PR` mode can create a PR. API-created sessions auto-approve the plan unless `requirePlanApproval=true` is set.

**Adapter assessment:** Jules is a practical candidate because it exposes an official REST interface rather than requiring browser automation. Alpha status requires schema/version guards and a fallback. AgentRelay must explicitly set plan approval behavior according to the user's policy; provider defaults must not silently bypass it. Jules session/activity history is not AgentRelay's canonical product state.

### GitHub Copilot dynamic workflows

GitHub documents dynamic workflows through the Copilot app, CLI and SDK. They support sequential/parallel steps, agent delegation, structured outputs, checkpoints and resumable runs. They are better treated as an **external workflow capability and competitor baseline** than as a simple coding-agent adapter.

Potential future integration:
- generate or maintain a repository workflow definition;
- trigger through a supported Copilot SDK/CLI route;
- import structured outputs and trace references;
- keep AgentRelay's goal/requirement/evidence records authoritative.

Before integrating, verify the exact SDK entry points, authentication, plan availability, cancellation semantics, data visibility and programmatic run-state access. Do not assume every local CLI control is exposed through a hosted API.

### GitHub Agentic Workflows

GitHub Agentic Workflows support Copilot, Claude, Codex and Gemini engines and are defined in Markdown with permissions, network configuration and safe outputs. Current docs describe read-only defaults, secrets held in downstream jobs, firewalled Actions environments and threat detection.

Prefer reusing these repository-native automations for recurring repo chores where they fit. AgentRelay should not rebuild an equivalent scheduler/security layer without a cross-system product-management requirement.

### Adapter requirements reinforced by this review

Every adapter must declare operation types, workspace ownership, repository/branch binding, plan approval defaults, follow-up/pause/resume/cancel/timeout support, structured and raw events, PR/branch side effects, authentication/billing/terms, quota, API stability and unsupported capabilities. Preserve raw provider IDs and events. Do not normalize providers into a lowest-common-denominator prompt interface.

### Sources reviewed 2026-11-02

- [Jules REST API quickstart](https://jules.google/docs/api/reference/)
- [Jules plan review](https://jules.google/docs/review-plan/)
- [Jules running tasks](https://jules.google/docs/running-tasks/)
- [Jules errors and failures](https://jules.google/docs/errors/)
- [GitHub Copilot dynamic workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/dynamic-workflows)
- [GitHub Agentic Workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows)

## 2026-11-03 integration refresh: Codex Agents API and workflow substrates

### OpenAI Agents API / managed Codex harness

OpenAI's current Agents API documentation describes a managed Codex harness. OpenAI manages sessions, orchestration, context compaction, and recovery; the application supplies tools and chooses the execution environment. Agents can operate in hosted or self-hosted sandboxes, execute code, edit files, connect to MCP servers, and produce artifacts. Model usage and tools are billed at applicable API rates.

For self-hosted execution, the documented architecture runs codex exec-server in a user-controlled laptop/container/remote sandbox. The executor establishes an outbound WebSocket connection and uses a separate restricted environment key; the application API key should remain outside the sandbox. The environment key can be read by generated code, so it is a scoped capability, not a harmless identifier.

This creates two Codex adapter paths:
1. **Local Codex CLI/app-server:** when the user wants the local agent harness and workspace to remain on their machine.
2. **Agents API + hosted/self-hosted executor:** when AgentRelay needs managed sessions, events, webhooks, and a separately provisioned execution environment, with API billing and OpenAI-managed harness behavior.

AgentRelay should not rebuild session management, context compaction, or agent tool loops if a supported harness already provides them. It should still own product requirements, cross-provider project state, evidence-to-requirement mapping, and product-level next-action decisions.

Sources: [OpenAI Agents API](https://developers.openai.com/api/docs/guides/agents-api/overview), [Codex as a platform](https://developers.openai.com/blog/codex-as-a-platform), [Self-hosted sandboxes](https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted), [Sandbox security](https://developers.openai.com/api/docs/guides/agents-api/environments/security).

### GitHub Agentic Workflows as an optional execution adapter

GitHub Agentic Workflows supplies a GitHub Actions-based substrate for agent workflows, including supported Copilot, Claude Code, Codex, and Gemini engines, deterministic steps, structured safe outputs, and security boundaries. AgentRelay should evaluate integration rather than reimplementing its workflow runner. A possible adapter could dispatch a repository workflow, correlate workflow/run IDs, consume structured artifacts and checks, and map results into AgentRelay's product ledger. This is an integration hypothesis; verify API behavior, permissions, preview status, and artifact availability before committing to it.

Source: [GitHub Agentic Workflows architecture](https://github.github.com/gh-aw/introduction/architecture/).

### Claude Agent SDK and Jules remain provider-specific

Claude Agent SDK documentation describes a programmable Claude Code harness in Python and TypeScript with built-in tools, permissions, sessions, hooks, approvals, structured output, cost/usage tracking, and OpenTelemetry. Keep the existing subscription-authentication/third-party distribution caveat: a local user's signed-in CLI use and a commercial product distributing subscription authentication are not automatically the same permission case.

Jules REST API sessions expose plan approval, activities, feedback and status. API-created sessions auto-approve plans unless requirePlanApproval=true; set it explicitly whenever AgentRelay's policy requires a plan gate. The API remains documented as v1alpha.

Sources: [Claude Agent SDK](https://code.claude.com/docs/en/agent-sdk/overview), [Claude SDK approvals](https://code.claude.com/docs/en/agent-sdk/handling-approvals), [Jules sessions API](https://jules.google/docs/api/reference/sessions).

### Adapter design consequence

The adapter contract must represent capability and policy differences, not just a common prompt/response: auth and billing route; local/provider-hosted/self-hosted execution; workspace isolation; session create/resume/fork/cancel; event cursor/replay; approval semantics; artifact/patch references; provider limits/model/usage/cost; durability/recovery; and exact permissions/secrets exposed to the executor.

Do not silently emulate an unsupported capability. Expose it as unsupported and let the planner select another executor or ask for intervention.


## 2026-11-04 integration update — Cursor, Claude Managed Agents, and provider semantics

### Cursor Cloud Agents API v1 (public beta)

Official reference: https://cursor.com/docs/cloud-agent/api/endpoints

- `POST /v1/agents` creates a durable Cloud Agent and enqueues its initial run.
- `POST /v1/agents/{id}/runs` sends a new prompt; delivery can be `steer` for an active turn or `queued` when no turn can accept it.
- `GET /v1/agents/{id}/runs` lists runs; `GET /v1/agents/{id}/runs/{runId}` retrieves status and terminal result.
- `GET /v1/agents/{id}/runs/{runId}/stream` emits SSE events. The stream is scoped to one run and does not replay prior runs.
- Create requests can choose a model, plan/agent mode, repositories and starting refs, cloud/pool/machine execution, environment variables, MCP servers, and custom subagents.
- Repositories can be connected from GitHub, GitLab, Bitbucket Cloud, and Azure DevOps.
- API v1 is public beta; docs say webhooks are coming soon (legacy v0 still documents them).

Adapter caveats:
- A durable agent can have multiple runs; store both IDs.
- Git branch/PR information returned in a run is documented as a per-agent snapshot, not per-run. Attribute work using run IDs and event data, then verify branch/commit state in the source-control provider.
- Because SSE does not replay prior runs, reconnect logic must retrieve run status and reconcile event history/artifacts where available.
- Cursor Automations are billed based on cloud-agent usage; capture actual usage and billing identity.
- Automation memories persist across runs, but Cursor warns that untrusted input can create misleading or malicious memories.

Related docs: https://cursor.com/docs/cloud-agent/automations · https://cursor.com/docs/cloud-agent/security-network · https://cursor.com/docs/cloud-agent/self-hosted

### Anthropic Claude Managed Agents (beta)

Official docs: https://platform.claude.com/docs/en/managed-agents/overview

Claude Managed Agents provides agent definitions, environments, stateful sessions, server-side event history, cloud or self-hosted sandboxes, memory stores, budgets, webhooks, scheduled deployments, multi-agent subagents, dynamic workflows, and outcome rubrics.

Key details:
- Requires the `managed-agents-2026-04-01` beta header and Claude API authentication.
- Dynamic workflows are agent-written programs that can run many agents in phases and pass results programmatically.
- A workflow can start in the same turn without a separate approval step.
- A workflow result of `completed` does not guarantee every child thread succeeded; inspect thread events.
- Outcome rubrics use a separate context-window grader and can drive iterative revisions; this is a bounded artifact/outcome loop, not proof of whole-product completion.
- Self-hosted sandboxes move tool execution to customer infrastructure, but tool inputs and outputs still flow through Managed Agents.
- The platform currently states it is not eligible for Zero Data Retention or HIPAA BAA coverage.

Additional docs: https://platform.claude.com/docs/en/managed-agents/multiagent-orchestration · https://platform.claude.com/docs/en/managed-agents/workflow-runs · https://platform.claude.com/docs/en/managed-agents/define-outcomes · https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes

### Claude Agent SDK local hosting

Official docs: https://code.claude.com/docs/en/agent-sdk/hosting

The SDK runs a Claude CLI subprocess per session. Transcripts, memory files, and workspace artifacts live on local disk by default and do not survive container restart or movement to another node. A SessionStore adapter is required for transcript persistence; memory and working-directory artifacts need separate storage. Third-party products may not offer claude.ai login or subscription rate limits without prior approval; use documented API-key auth unless approved.

### OpenAI Codex app-server / SDK

OpenAI's August 19, 2026 platform post recommends `codex exec` for bounded jobs, the Codex SDK for programmatic workflows, and app-server for persistent conversations, streaming, interruption, application tools, and approval handling. The host application owns its domain context, UI, tools, execution environment, and policy boundaries.

### Normalize semantics, not just endpoint shapes

Each adapter capability profile should record support and maturity for create, follow-up, steer, queue, pause, cancel, resume, event stream, replay, webhooks, polling, artifacts, Git attribution, cost, budgets, sandbox, network, secrets, and approval callbacks.

Keep `provider_run_status` separate from `task_acceptance_status`. A successful provider terminal status is never sufficient to mark a requirement as verified.
