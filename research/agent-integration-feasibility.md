# Coding-Agent Integration Feasibility

**Reviewed:** 2026-10-31  
**Status:** Initial feasibility scan; validate exact versions, auth, terms, and runtime behavior before implementation.

## Executive conclusion

There are workable programmatic integration paths. AgentRelay should prefer official SDKs, headless CLIs, hosted-agent APIs, or documented protocols. Browser/IDE GUI automation should be a last resort.

An adapter must declare its real capabilities. Do not pretend all agents support identical sessions, streaming, cancellation, permission callbacks, workspace control, or usage reporting.

## Provider and runtime scan

| Agent/runtime | Publicly documented integration surface | Useful facts | Main caveats |
|---|---|---|---|
| Claude Code / Claude Agent SDK | Official Python and TypeScript Agent SDK; CLI subprocess with non-interactive mode and JSON output | Reuses Claude Code's agent loop, tools, permissions, sessions, hooks, subagents, MCP, structured output, and observability features. | Third-party products must not assume they can offer Claude.ai login or subscription rate limits; Anthropic says to use API-key authentication unless separately approved. Consumer-plan usage and API credits are distinct. |
| OpenAI Codex | Codex CLI and repeatable codex exec workflows; local repository access, permission controls, model selection, scripts and CI | Suitable for local agent execution and automation. | Pin versions and test event/output contracts. Keep credentials out of logs and repository files. Deep app-server integration should be evaluated against official current documentation and compatibility policy. |
| Gemini CLI | Headless mode with JSON and JSONL output; session and plan features documented | Provides structured output, tool-use events, token/latency statistics, and exit codes. | Pin CLI version; verify auth, session resume, cancellation, and sandbox behavior for the chosen workflow. |
| Gemini API CLI | Stateless shell client around the Interactions API | Can be called by scripts and coding agents; structured output and background managed-agent commands are documented. | It is an API client, not a coding-agent runtime. API billing and model availability apply. |
| GitHub Copilot cloud agent | GitHub-hosted task/issue workflows; third-party Claude and Codex agents documented in public preview | Creates PRs and supports follow-up iteration through review. | GitHub-centric and policy/billing-dependent; not a generic local agent process. Check repository and branch scope limitations. |
| OpenHands Software Agent SDK | Python, TypeScript, REST, Agent Server | Local or ephemeral Docker/Kubernetes workspaces; suitable for building or embedding coding agents. | AgentRelay would still need product planning, project state, policy, and verification. |
| Devin | Product sessions and coding workflows; public product/API surfaces need deeper inspection | Potential hosted executor for software tasks. | Confirm API access, task lifecycle, workspace boundaries, pricing, and terms before adapter design. |
| Cursor / Windsurf / other IDE agents | Product-specific background agents, extensions, or CLI capabilities | May be useful to developers in their native environment. | Avoid GUI automation. Verify stable public interfaces, licensing, and supported control semantics per product. |
| A2A-compatible remote agents | A2A task/message/artifact protocol | Standard discovery and remote task communication where implemented. | A local CLI is not automatically an A2A server. Protocol support must be confirmed for each agent. |

## Important Anthropic constraint

The current [Claude Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview) says the SDK is intended for embedding Claude Code's agent in a developer-operated Python or TypeScript process, and that third-party developers may not offer Claude.ai login or subscription rate limits for their products unless previously approved. It directs developers to API-key authentication and states that SDK use is governed by Anthropic's Commercial Terms of Service.

Anthropic's [monthly API credit guidance](https://support.claude.com/en/articles/17154008-monthly-api-credits-for-max-ultra-and-team-plans) says API credits are available on Max, Ultra, and Team plans, not Pro. It also distinguishes API-key-backed Agent SDK use from usage that draws on a signed-in Claude plan.

**Product implication:** do not design AgentRelay as a service that silently consumes a user's Claude Pro login or resells their subscription limits. For productized Agent SDK integration, use an approved API-key or commercial integration path. A local, user-operated CLI workflow should still be reviewed against the exact current terms before it is automated or distributed. This is a launch blocker to resolve, not a minor implementation detail.

## Adapter capability contract

Each adapter should publish a machine-readable capability profile containing:
- provider and product name;
- adapter version and supported product versions;
- authentication mode and who owns the credentials;
- local, hosted, or remote execution model;
- workspace and repository scope;
- file, shell, browser, and network capabilities;
- structured output schema and event stream format;
- start, follow-up, resume, fork, cancel, and timeout support;
- permission modes, hooks, and approval callbacks;
- usage, token, cost, and latency reporting;
- supported operating systems and prerequisites;
- rate limits, known restrictions, and terms links.

AgentRelay should not promise a capability simply because another provider offers it.

## Recommended adapter order

1. **First adapter:** choose the most stable and affordable official interface that the builder can legally use. Claude Agent SDK is technically capable, but its API-key/terms model must be budgeted and resolved.
2. **Second adapter:** add Codex or Gemini CLI after the end-to-end manager loop works.
3. **Alternative execution runtime:** test OpenHands for isolated workspaces and agent execution.
4. **Hosted agent integrations:** add GitHub Copilot, Devin, or other hosted agents when their APIs and policies fit a specific user workflow.
5. **Remote agents:** add A2A only when remote agent interoperability is needed.

## Adapter conformance tests

Every adapter should pass tests for correct workspace and repository binding; clean start, streaming, and structured final result; non-zero exit and provider error handling; timeout and cancellation; follow-up and session resume if claimed; permission denial and approval callback behavior; output capture and secret redaction; cost/usage reporting or explicit unsupported status; crash recovery without duplicate external side effects; and version compatibility with a documented fallback.

## Sources reviewed on 2026-10-31

- [Claude Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview)
- [Anthropic monthly API credits and Agent SDK usage](https://support.claude.com/en/articles/17154008-monthly-api-credits-for-max-ultra-and-team-plans)
- [Codex CLI](https://developers.openai.com/codex/cli)
- [Gemini CLI headless mode](https://geminicli.com/docs/cli/headless/)
- [Gemini API CLI](https://ai.google.dev/gemini-api/docs/cli)
- [GitHub third-party coding agents](https://docs.github.com/en/copilot/concepts/agent-capabilities/about-agents/about-third-party-coding-agents)
- [OpenHands Software Agent SDK](https://github.com/OpenHands/software-agent-sdk)
- [A2A core concepts](https://a2a-protocol.org/latest/topics/key-concepts/)
