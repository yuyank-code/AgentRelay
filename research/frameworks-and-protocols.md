# Frameworks and Protocols — What AgentRelay Should Reuse

**Reviewed:** 2026-10-31  
**Purpose:** Distinguish agent orchestration, durable workflow execution, tool access, and remote agent interoperability.

## Executive summary

These technologies solve different layers. AgentRelay should not bundle them all by default.

- A workflow/agent framework helps implement reasoning and delegation.
- A durable workflow engine helps long-running work survive failures and wait for people or external events.
- MCP connects agents to tools and contextual resources.
- A2A lets independently implemented agents communicate.
- A coding-agent runtime executes software work in a repository or sandbox.
- AgentRelay must still own its product-goal, requirements, decisions, evidence, policy, and user-facing lifecycle model.

## Comparison

| Technology | Main abstraction | Useful capabilities | What AgentRelay still owns |
|---|---|---|---|
| LangGraph | Stateful graph of agent/application steps | Checkpoints, persistence, human interrupts, recovery from failed nodes, time-travel debugging, conversational memory | Product requirements, project state, goal progress, evidence provenance, cross-provider product lifecycle, policies |
| Microsoft Agent Framework (MAF) | Agent harness and multi-agent workflows | Python/.NET agent runtime, tools, MCP, middleware, long-running context, approval flows, background agents, observability | Product-level domain model and goal-to-release evidence |
| AutoGen | Agents and teams | AgentChat teams, human-in-the-loop patterns, save/load of agent/team state, GraphFlow workflows | Durable project truth and external side-effect reconciliation |
| CrewAI | Crews and Flows | Role/task composition, flow state, persistence and resume, sequential/conditional workflow patterns | Product-state schema, requirements traceability, evidence and release lifecycle |
| Google ADK 2.0 | Agent and graph workflow runtime | Graph-based, dynamic, and collaborative workflows; Python, TypeScript, and Go | Vendor-neutral project and product model |
| OpenHands SDK / Agent Server | Software-agent runtime and workspace API | Python, TypeScript, REST; local or ephemeral Docker/Kubernetes workspaces; coding tools and conversations | The manager/control plane that decides what to build and verifies goal progress |
| Temporal | Durable workflow execution | Event history, replay, recovery, long-running workflows, external signals and workflow lifecycle | LLM planning and product domain decisions |
| A2A | Agent-to-agent protocol | Agent Cards, task lifecycle, messages, artifacts, polling, SSE streaming, push notifications, context IDs | Product requirements, code provenance, test policy, product goal, and release approvals |
| MCP | Agent-to-tool/context protocol | Resources, prompts, tools, progress, cancellation, logging, and capability negotiation | Agent selection, product lifecycle, cross-agent orchestration, project policy |

## Findings

### LangGraph

LangGraph checkpointers save graph state across steps. The official documentation describes human-in-the-loop inspection and approval, conversational memory, time travel, and fault tolerance. This is valuable for implementing a reasoning loop that can pause and resume.

It does not automatically provide AgentRelay's product model. A persisted graph thread is not the same thing as a versioned product specification, a repository evidence ledger, or a verified release.

**Recommendation:** prototype the goal-assessment/replanning subgraph in LangGraph if branching and interrupts are useful. Keep the domain model outside the graph and avoid using chat thread state as the canonical project record.

Source: [LangGraph checkpointers](https://docs.langchain.com/oss/python/langgraph/persistence).

### Microsoft Agent Framework and AutoGen

Microsoft announced Agent Framework 1.0 GA on 2026-04-02 as a convergence of AutoGen and Semantic Kernel. Its 2026 documentation describes an agent harness with context management, tools, human approval, background agents, and observability. AutoGen AgentChat separately documents saving and loading agent and team state.

**Recommendation:** evaluate the current Microsoft Agent Framework rather than beginning a new project on an older AutoGen-specific architecture if a Microsoft ecosystem is relevant. Neither framework removes the need for an independent product state and external-effect ledger.

Sources: [Microsoft Agent Framework announcement, 2026-06-03](https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-at-build-2026-announce/), [AutoGen state management](https://microsoft.github.io/autogen/dev/user-guide/agentchat-user-guide/tutorial/state.html).

### CrewAI

CrewAI separates structured Flow control from Crews used for autonomous work. Its documentation describes structured state and persisted flow state that can be paused, resumed, and recovered.

**Recommendation:** useful for quickly experimenting with role-based discovery or research teams, but do not assume role-playing agents are required. The product manager loop may be simpler and more reliable as a small number of typed decisions plus deterministic workflow transitions.

Source: [CrewAI Flow state management](https://github.com/crewAIInc/crewAI/blob/main/docs/v1.10.1/en/guides/flows/mastering-flow-state.mdx).

### Google ADK 2.0

ADK 2.0 documents graph-based, dynamic, and collaborative workflows across Python, TypeScript, and Go. Its workflow runtime is an alternative for teams that want a Google-aligned agent stack.

**Recommendation:** keep it as an alternative; do not adopt it simply because AgentRelay may use Gemini as one provider.

Source: [Google ADK 2.0](https://github.com/google/adk-docs/blob/main/docs/2.0/index.md).

### OpenHands

The OpenHands Software Agent SDK provides Python, TypeScript, and REST APIs. It supports local workspaces and ephemeral workspaces such as Docker or Kubernetes through the Agent Server. It is a candidate for the execution plane, not a substitute for the product manager/control plane.

**Recommendation:** compare OpenHands against provider-native CLIs for the first execution adapter. It may reduce the amount of runtime and workspace infrastructure AgentRelay needs to write itself.

Source: [OpenHands Software Agent SDK](https://github.com/OpenHands/software-agent-sdk).

### Temporal versus LangGraph

Temporal documents workflows that can run for long periods and recover after infrastructure failure by replaying recorded event history. LangGraph checkpoints state within an agentic graph and supports human interrupts and recovery.

These can complement one another, but using both adds operational complexity.

**Recommendation:** first write the actual durability requirements. If the first milestone is a local workflow, an explicit state machine or LangGraph may be enough. If projects must run for days or weeks, wait for human input, survive worker loss, and safely resume external actions, evaluate Temporal. Do not combine both until a concrete need justifies it.

Source: [Temporal workflow documentation](https://docs.temporal.io/workflows).

### A2A

A2A's latest documentation defines Agent Cards, stateful tasks, messages, parts, artifacts, context IDs, polling, SSE streaming, and push notifications. It is relevant when AgentRelay needs to discover and communicate with remote agent servers.

A2A is not a complete software project manager. It does not define AgentRelay's product goal, requirement graph, Git provenance, test acceptance policy, or release approval model.

**Recommendation:** keep A2A behind an adapter for remote agents. Do not require every local coding CLI to expose an A2A endpoint.

Source: [A2A core concepts](https://a2a-protocol.org/latest/topics/key-concepts/).

### MCP

MCP standardizes how LLM applications connect to external context and tools. Its specification includes resources, prompts, tools, progress, cancellation, error reporting, and logging. Its security guidance emphasizes user consent, data privacy, and tool safety.

MCP is not agent-to-agent communication and does not define a project lifecycle.

**Recommendation:** use MCP to expose selected project data and approved tools to agents. Keep authorization and policy enforcement in AgentRelay and the execution environment; do not assume protocol support alone makes a tool safe.

Source: [MCP specification, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25).

## Architecture decision guidance

1. Own the product and evidence domain model.
2. Use one workflow engine initially.
3. Use provider-native SDKs/CLIs or OpenHands for execution.
4. Use MCP for tool access where it helps.
5. Add A2A for remote agents only when a real integration requires it.
6. Re-evaluate frameworks when pause/resume, concurrency, approval, idempotency, or observability requirements become concrete.
