# Competitive Landscape — Initial Scan

**Reviewed:** 2026-10-30  
**Confidence:** Preliminary; requires deeper product-by-product validation.

## 1. Categories that overlap with AgentRelay

### Agent orchestration frameworks
Examples: LangGraph, Microsoft Agent Framework, Google Agent Development Kit, CrewAI, Agno, AutoGen/AG2.

They provide primitives for defining agents, tools, state, delegation, workflows, and human-in-the-loop behavior. These are potential implementation dependencies or alternatives for the orchestration layer.

**Implication:** AgentRelay should not differentiate merely by routing tasks between LLMs or implementing a graph of agents.

### Agent interoperability protocols
- **A2A:** agent-to-agent interoperability, discovery, task delegation, and result exchange across different agent implementations.
- **MCP:** standardized connections between an agent and tools/resources.

The official A2A documentation explicitly positions A2A and MCP as complementary, and A2A as a communication protocol rather than an agent development framework.

**Implication:** Evaluate A2A for interoperable agent endpoints and MCP for repository/tool access. Neither alone defines a durable software-project state model or a complete coding-task verification lifecycle.

### Coding agents and developer environments
Examples to investigate: Claude Code, OpenAI coding agents/Codex, Gemini CLI, GitHub Copilot coding agent, Cursor, Windsurf, OpenHands, Cline, Aider.

These tools increasingly provide their own planning, editing, context, tool use, and review workflows.

**Implication:** Adapter feasibility, official automation interfaces, session resume, workspace control, and permission semantics are critical research items. Do not assume a web UI can or should be automated.

### Emerging wrappers and gateways
A preliminary scan surfaced community projects that expose coding agents through A2A-compatible interfaces. This is a direct competitive signal: simply wrapping coding CLIs in A2A may not be a defensible differentiator.

**Implication:** AgentRelay must go beyond protocol wrapping into project continuity, evidence, policy, recovery, and UX—or discover a more valuable wedge.

## 2. Initial differentiation hypothesis

Potential product-owned capabilities:
1. Canonical project/task state independent of provider conversation history.
2. Context compilation grounded in Git commits, diffs, tests, docs, and decisions.
3. Explicit handoff contracts with acceptance criteria and action permissions.
4. Evidence-backed completion and cross-agent review.
5. Safe workspace isolation and conflict management.
6. Recovery and resumability when an agent crashes, times out, or becomes unavailable.
7. A unified developer timeline across tools.

These are hypotheses, not proven gaps. Competitor research must test whether existing products already deliver them.

## 3. Sources reviewed

- A2A official documentation: https://a2a-protocol.org/latest/ — describes A2A interoperability and its relationship to MCP; accessed 2026-10-30.
- A2A official GitHub documentation: https://github.com/a2aproject/A2A — protocol specification and examples; accessed 2026-10-30.
- Microsoft Learn, agent-to-agent communication: https://learn.microsoft.com/en-us/microsoftteams/platform/agents-in-teams/integrate-ai/agent-to-agent — demonstrates peer-agent handoff; accessed 2026-10-30.
- Community A2A coding-agent wrapper surfaced in initial scan: https://a2a-wrapper.allsrc.dev/docs — illustrates that exposing coding agents through A2A is already being explored; community project, not proof of vendor-supported integrations; accessed 2026-10-30.
- LangGraph documentation: https://docs.langchain.com/oss/python/langgraph/overview — review required for detailed feature comparison.

## 4. Gaps to investigate next

- Compare exact project/session continuity features in Claude Code, Codex, Gemini CLI, Cursor, Copilot, OpenHands, Cline, and Aider.
- Verify supported programmatic interfaces, licenses, rate limits, and commercial-use constraints.
- Determine which agents support headless execution, structured output, session resume, streaming, cancellation, and workspace isolation.
- Compare A2A task/artifact semantics with AgentRelay's proposed task and evidence model.
- Review current products that coordinate coding agents or transfer tasks between them.
- Interview developers who regularly switch between coding agents.
