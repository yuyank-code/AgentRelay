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
