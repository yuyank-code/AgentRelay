# Research Update — 2026-11-01

**Run date:** 2026-11-01  
**Repository baseline:** main at da1e260f39d63c675724ac67637c6c4e3b272979  
**Scope:** competitor refresh, current agent integration surfaces, subscription/auth nuance, execution-harness strategy, and evaluation implications.

## Executive summary

The most important result is not that AgentRelay needs more agents. It is that the boundary between a product-level manager and a coding-agent harness is now especially important.

- Shep's current public workflow goes beyond parallel CLIs: it advertises optional research, requirements, and approved plans, isolated worktrees, CI repair retries, human question routing, checkpoints, and PR delivery.
- GitHub now documents both third-party Claude/Codex coding agents and GitHub Agentic Workflows that can run Copilot, Claude, Codex, or Gemini from Markdown-defined repository automations with safety controls.
- Factory's public Droids positioning directly overlaps with model routing, codebase understanding, adjustable autonomy, persistent memory, and one-prompt-to-PR.
- OpenHands has a separate Automation Service for cron/webhook dispatch and run history alongside its Software Agent SDK and Agent Server.
- OpenAI's Codex platform direction makes its agent harness reusable through CLI, SDK, and app-server. This is a strong reason not to build another generic agent loop.
- Anthropic's official Agent SDK overview and Help Center article contain a meaningful scope/permission tension around third-party subscription-backed use. The repository's previous categorical warning was too broad and has been corrected.

The possible AgentRelay wedge remains product-level goal-to-evidence management across discovery, requirements, implementation, release, and operational feedback. This is not proven unique or commercially validated.

## 1. Repository inspection and gap assessment

At the start of this run, main pointed to da1e260f39d63c675724ac67637c6c4e3b272979. The repo already contained a goal-oriented product vision, two-plane architecture, competitive scan, integration scan, evaluation plan, security model, and two ADRs.

The biggest gaps were:
1. the competitor scan understated the depth of Shep's current feature/PR workflow;
2. it did not include GitHub Agentic Workflows as a multi-provider repository automation baseline;
3. it did not fully account for OpenHands Automation Service;
4. it did not make the manager-versus-agent-harness boundary explicit enough;
5. the Claude subscription warning needed correction and qualification against current official documentation.

No application code or unrelated files were changed.

## 2. Competitive findings

### Shep

The current [Shep product page](https://shep.bot/) describes a full feature-level pipeline: one-line feature intake; optional spec-driven research, requirements, and plan in YAML with approval; multiple coding CLIs; a Git worktree per feature; CI watching and configurable fix retries; human questions with recommended answers; checkpoints after agent steps; dashboard, CLI, and TUI; and PR delivery with the human retaining merge control by default.

This is a close competitor to the initial AgentRelay proposal. It already covers many features that might otherwise be described as differentiation: provider choice, worktree isolation, task plans, review gates, bounded retries, checkpoints, and human triage.

The public documentation reviewed does not establish whether Shep maintains a durable, multi-feature product model connecting original goals to changing requirements, releases, and runtime feedback over long periods. That is a testable unknown, not an asserted missing feature.

### GitHub

GitHub documents third-party Claude and Codex coding agents in public preview. They can be started from repository agent surfaces, issues, PRs, mobile, and VS Code; agents can create PRs and iterate through review comments.

Separately, GitHub Agentic Workflows turns Markdown instructions into GitHub Actions workflows and supports Copilot, Claude, Codex, and Gemini. Official docs describe read-only-by-default permissions, declared safe outputs, secrets isolated from the agent runtime, firewalled execution, threat detection, role controls, and usage/cost audit commands.

This is an important baseline because the user may not need AgentRelay for common repository automations. GitHub Agentic Workflows are repository-scoped and trigger-based in the documented model; AgentRelay's candidate value must be evaluated at the broader product level and across external systems, not merely as another way to run a coding agent on a repo.

### Factory

Factory's Droids page claims that agents can plan, write, test, and ship code; route across Claude, GPT, and Gemini; understand large codebases; adjust autonomy; maintain memory across tools; and deliver from one prompt to a PR.

This directly overlaps with much of the broad AgentRelay vision. Vendor claims are not independent performance evidence, but they are enough to rule out a simplistic positioning such as "a model-independent AI manager."

### OpenHands

The OpenHands Software Agent SDK exposes Python, TypeScript, and REST APIs, local or ephemeral Docker/Kubernetes workspaces, conversations, tools, and events. A separate Automation Service manages cron schedules, webhooks, run history, dispatch, and sandbox lifecycle; its repository marks it beta.

OpenHands can potentially supply parts of the execution plane. AgentRelay should investigate reuse before implementing its own workspace runtime and scheduler.

### Codex

OpenAI's 2026-08-19 Codex platform article describes an open harness that manages the agent loop, context, tools, sandbox/approval policies, streaming, and continuation. It documents three integration levels: codex exec for bounded jobs, the SDK for application-level task control, and app-server for persistent threads, events, interruption, tools, and approvals.

The article explicitly places the application interface, domain context, tools, and operational boundaries in the host product. This supports the proposed AgentRelay split: manager/control plane outside, execution harnesses underneath.

## 3. Agent integration and authentication findings

### Claude Agent SDK: correct the earlier blanket conclusion

The [Claude Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview) says third-party developers may not offer Claude.ai login or subscription rate limits for their products unless previously approved. It recommends API-key authentication for product builders and says the SDK is governed by Anthropic's Commercial Terms.

However, the [Help Center article](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan) includes updates dated 2026-06-15 and 2026-10-07. It says the Agent SDK, claude -p, and third-party app usage still draw from subscription limits when signed in with a Claude plan. It also says Max, Ultra, and Team plans include monthly API credits for API-key use through a linked Console organization; Pro is not listed among those API-credit plans.

These official statements should not be collapsed into a blanket permission. The support article describes plan-backed usage, while the SDK overview restricts third-party products from offering Claude.ai login or subscription rate limits without approval. AgentRelay must obtain written clarification for its intended distribution model before promising this as a supported product feature. For a local user-operated experiment, the support article is relevant, but it does not remove the need to follow the SDK terms.

### Codex

The Codex app-server provides a documented route for an application to create threads, start turns, receive events, interrupt work, expose tools, and handle approval requests. The CLI, SDK, and app-server serve different integration depths; the adapter should choose based on whether the task is a one-off command or a persistent in-product session.

### Gemini

Gemini CLI headless mode provides structured JSON/JSONL outputs and typed exit codes. Gemini API CLI is a separate stateless shell client for the Interactions API, with background managed agents, explicit interaction chaining, and structured error reporting. It can be useful for research and specialist API tasks, but it is not itself a coding-agent runtime.

### GitHub and OpenHands

GitHub Agentic Workflows can execute several third-party coding CLIs inside Actions, with provider-specific credentials. OpenHands SDK and Automation Service offer an alternative self-hostable execution and scheduling stack. Each path has different auth, billing, workspace, and operational constraints.

## 4. Architecture implication: AgentRelay is the manager, not the harness

Do not rebuild the generic tool-calling loop if a provider already supplies one.

AgentRelay should own:
- the user's original product goal and approved scope;
- versioned requirements, assumptions, constraints, and success measures;
- architecture decisions, roadmap, dependencies, and task selection;
- cross-agent continuity and task-specific context compilation;
- requirement-level verification status and evidence provenance;
- cross-system release state and eventual runtime feedback;
- autonomy policies, budgets, approval, escalation, and audit.

The provider harness should own its native model/tool loop, provider-specific context management, local/remote tool execution, session mechanics, streaming, and runtime permission hooks.

The adapter should expose a capability profile and normalize only the outer lifecycle. Preserve raw events, logs, artifacts, session IDs, and unsupported-capability declarations. Never imply that every provider supports identical resume, cancellation, approval, sandbox, or cost APIs.

The manager's LLM can propose the next action, but a deterministic policy layer authorizes it. The durable workflow engine records state transitions and reconciles external side effects. The execution runtime enforces workspace and process boundaries.

## 5. Updated differentiation hypothesis

Weak or commoditized claims:
- multiple coding agents in one UI;
- model routing;
- handoff packets and persistent agent sessions;
- plan/spec generation for a feature;
- worktrees, CI watching, retries, checkpoints, and PRs;
- approval gates and audit logs;
- natural-language repository automations.

Candidate claim to test:
- a durable product-level control plane that maintains a traceable relationship from the original goal through requirements, assumptions, architecture decisions, implementation, tests, releases, and operational signals;
- uses that evidence to identify the most valuable next action and revise the plan without silently changing the user's goal;
- works across providers and external systems rather than being tied to one agent vendor or one repository workflow.

This remains a hypothesis. Factory and enterprise workflow products make broad adjacent claims, and the desk research did not establish a defensible moat.

## 6. Evaluation changes

The baseline set should include, where access permits:
1. one strong single-agent workflow with project instructions and human steering;
2. Shep;
3. GitHub Agentic Workflows and/or GitHub Copilot third-party agents;
4. Factory Droids;
5. AgentRelay.

The task suite should test goal drift, requirement evolution, cross-system dependencies, task-success versus product-success, provider substitution, and post-release feedback. Primary metrics should be time and cost to a verified product outcome and human coordination effort—not agent count, generated tasks, or PR volume.

## 7. Recommended next steps

1. Run a hands-on comparison of Shep, GitHub Agentic Workflows, and Factory on the same 5–10-step product task chain.
2. Interview 10–15 developers who actually use coding agents on multi-week projects; ask about recent work and current tool failures, not hypothetical interest.
3. Test the Claude Agent SDK's local user-operated auth path and obtain written Anthropic clarification before productizing any subscription-backed login.
4. Prototype one native harness integration and measure how much manager logic must remain outside the harness.
5. Freeze a project-level evaluation suite before expanding adapters or parallel execution.

## 8. Sources reviewed on 2026-11-01

- [Shep product page](https://shep.bot/)
- [Shep documentation](https://shep.bot/docs)
- [GitHub third-party coding agents](https://docs.github.com/en/copilot/concepts/agent-capabilities/about-agents/about-third-party-coding-agents)
- [GitHub Agentic Workflows overview](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows)
- [GitHub Agentic Workflows in Actions](https://docs.github.com/en/actions/tutorials/develop-agentic-workflows-in-github-actions)
- [Factory Droids](https://factory.com/product/droids)
- [OpenHands Software Agent SDK](https://github.com/OpenHands/software-agent-sdk)
- [OpenHands Automation Service](https://github.com/OpenHands/automation)
- [Codex as a platform, 2026-08-19](https://developers.openai.com/blog/codex-as-a-platform)
- [Claude Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview)
- [Claude Agent SDK with Claude plan](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)
- [Gemini CLI headless mode](https://geminicli.com/docs/cli/headless/)
- [Gemini API CLI](https://ai.google.dev/gemini-api/docs/cli)

## 9. Open questions

- Does Factory already deliver the product-level requirement/release/operations model AgentRelay wants, in practice?
- Does Shep's durable project state span more than feature/PR workflows?
- Can GitHub Agentic Workflows be composed into a long-running product manager, or are external orchestration and product state still needed?
- What is the smallest real workflow where AgentRelay measurably reduces human coordination beyond a strong single agent?
- Which auth modes are permitted for a distributed product across each provider?
- Does AgentRelay need its own execution runtime, or can it remain a control plane over Codex, Claude, OpenHands, and other harnesses?
