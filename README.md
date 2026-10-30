# AgentRelay

**Universal AI Development Handoff Platform**

AgentRelay aims to let a developer move ongoing software work between different AI models and coding environments—such as Claude Code, OpenAI-powered coding agents, Gemini-based agents, and other supported tools—without manually reconstructing project context and next-step instructions.

## The problem

AI coding tools can independently plan, edit, test, and review code, but a developer who switches tools often has to manually transfer:
- the project goal and constraints;
- current implementation state;
- decisions and rejected alternatives;
- files changed and commands/tests run;
- known failures and unresolved questions;
- the next concrete task and its acceptance criteria.

AgentRelay investigates and prototypes a reliable, auditable handoff layer around existing coding agents.

## Product thesis

**The project—not the conversation—is the durable unit of work.**

AgentRelay should maintain a provider-neutral project record grounded in repository state, task history, test evidence, decisions, and explicit handoff artifacts. It should be able to prepare a task for one agent, collect its work, verify the result, and route the next task to an appropriate agent.

AgentRelay is not intended to reimplement LangGraph, A2A, MCP, or coding agents. It should integrate with established tools where they fit and focus its product value on project continuity, coding-environment adapters, evidence-based handoffs, policy controls, and developer experience.

## Initial principles

1. **Repository-grounded context:** Git and the working tree are evidence; model conversation history is not the source of truth.
2. **Provider-neutral core:** project state and task artifacts must not depend on one model vendor.
3. **Explicit handoff contract:** every handoff includes objective, context, constraints, expected outputs, acceptance criteria, and permissions.
4. **Verify before advancing:** task completion is not accepted merely because an agent says it is done.
5. **Human control:** risky actions, destructive changes, external side effects, and merges require configurable approval.
6. **Least privilege:** agents receive only the repository, tools, secrets, and permissions needed for the assigned task.
7. **Observable execution:** record who/what acted, what changed, which checks ran, and why the workflow advanced.
8. **Incremental adoption:** work with one local repository and two agent adapters before attempting a universal agent marketplace.

## Status

Research and product definition. No production implementation is claimed yet.

## Repository map

- [Product brief](docs/product-brief.md)
- [Initial architecture](architecture/initial-architecture.md)
- [Research overview](research/README.md)
- [Competitive landscape](research/competitive-landscape.md)
- [Security and trust model](docs/security-and-trust.md)
- [Roadmap](roadmap/roadmap.md)
- [ADR-0001: Build a product layer, not another orchestration framework](decisions/ADR-0001-product-layer.md)

## Suggested first milestone

Build a local proof of concept that:
1. registers one Git repository as a project;
2. captures a structured project snapshot;
3. creates a bounded task with acceptance criteria;
4. dispatches it to one coding-agent adapter;
5. collects the diff, logs, and test evidence;
6. generates a handoff packet for a second agent;
7. pauses for human approval before consequential actions.

## Research sources

- [A2A Protocol](https://a2a-protocol.org/latest/)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [LangGraph documentation](https://docs.langchain.com/oss/python/langgraph/overview)
- [OpenAI Agents SDK](https://openai.github.io/openai-agents-python/)
- [Anthropic Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code/overview)

See the research directory for dated analysis and open questions.
