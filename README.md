# AgentRelay

**An AI system for managing software development from idea to release.**

AgentRelay is intended to let a person describe the software outcome they want, then have an AI development manager coordinate the research, product definition, planning, implementation, verification, release preparation, and ongoing improvement needed to reach that outcome.

The person remains the product owner: they set goals and boundaries, review important decisions, and intervene when human judgment is required. AgentRelay manages the day-to-day development loop and uses existing AI models, coding agents, developer tools, and execution environments as its workforce.

## The problem

AI can write code, but people still do much of the work around the code: translating a broad idea into requirements and architecture; deciding what should be built next; moving context between sessions, models, agents, and tools; checking whether completed work satisfies the product goal; diagnosing failures; and coordinating tests, reviews, releases, and maintenance.

A coding agent usually receives a bounded task. AgentRelay's intended responsibility is broader: keep the complete product goal in view and continuously manage progress toward it.

## Product thesis

**The product goal and its evidence—not an individual conversation—are the durable unit of work.**

AgentRelay should maintain a versioned, provider-neutral model of the product: goals, users, requirements, constraints, decisions, architecture, workstreams, tasks, dependencies, repository state, verification evidence, releases, and operational feedback.

Its central loop is:

**Discover → Define → Plan → Execute → Verify → Replan → Release → Observe → Improve**

The loop should continue without requiring the user to manually write every next instruction. It must pause or request approval when requirements are ambiguous, risks exceed policy, or human judgment is needed.

## What AgentRelay owns

1. **Product understanding:** turn an initial idea into clarified goals, requirements, assumptions, constraints, and measurable success criteria.
2. **Development strategy:** create and revise architecture, milestones, dependencies, and implementation plans.
3. **Project continuity:** preserve decisions, current state, prior attempts, and evidence across sessions and providers.
4. **Agent coordination:** select and dispatch suitable coding agents through supported interfaces.
5. **Quality and verification:** connect requirements to tests, code changes, reviews, security checks, and release evidence.
6. **Recovery and replanning:** respond to failures, changing requirements, unavailable agents, and newly discovered dependencies.
7. **Human control:** present meaningful decisions, approvals, blockers, and progress without hiding consequential actions.
8. **Lifecycle feedback:** eventually connect releases and runtime signals back into maintenance and improvement work.

## What AgentRelay is not

- Not merely a chat interface or prompt-forwarding service.
- Not just a task board or generic multi-agent framework.
- Not a replacement for foundation models, coding agents, Git, CI, or deployment platforms.
- Not an unrestricted autonomous system with permission to change production systems silently.
- Not a claim that every vendor permits its consumer subscription to be automated by third-party software.

## Product principles

- **Goal-oriented, not prompt-oriented:** measure progress against the intended product outcome.
- **Human remains the product owner:** automate routine execution, not accountability.
- **Evidence before status:** an agent's claim is not proof that a requirement is satisfied.
- **Versioned project memory:** preserve why decisions were made and which evidence supports them.
- **Dynamic plans:** plans are hypotheses that change when reality changes.
- **Provider-neutral, capability-aware:** integrate multiple providers without pretending their interfaces and permissions are identical.
- **Least privilege and safe execution:** isolate workspaces, scope credentials, and gate risky actions.
- **Observable and recoverable:** every important action, state transition, approval, and failure is traceable.
- **Use existing infrastructure:** adopt workflow engines and protocols where they solve real problems.

## Status

Research and product definition. No production implementation is claimed.

## Repository map

- [Product vision](docs/product-vision.md)
- [Product brief](docs/product-brief.md)
- [Initial architecture](architecture/initial-architecture.md)
- [Research overview](research/README.md)
- [Competitive landscape](research/competitive-landscape.md)
- [Frameworks and protocols](research/frameworks-and-protocols.md)
- [Agent integration feasibility](research/agent-integration-feasibility.md)
- [Dated research update](research/2026-10-31-research-update.md)
- [Evaluation plan](research/evaluation-plan.md)
- [Security and trust](docs/security-and-trust.md)
- [Roadmap](roadmap/roadmap.md)
- [ADR-0001](decisions/ADR-0001-product-layer.md)
- [ADR-0002](decisions/ADR-0002-goal-oriented-manager.md)

## First proof milestone

The first milestone should be a small but complete vertical slice of the intended product—not merely a CLI that forwards a task:

1. A user describes a small software product.
2. AgentRelay asks the important clarifying questions and creates a reviewable product brief.
3. It creates a versioned requirement set, architecture outline, and dependent development plan.
4. It dispatches implementation work to one supported coding-agent adapter in an isolated workspace.
5. It captures actual diffs, command results, tests, and review evidence.
6. It maps evidence back to requirements, detects gaps, and decides whether to accept, retry, replan, or escalate.
7. It starts the next appropriate task without manual prompt reconstruction.
8. It presents the project state and asks for approval before consequential actions such as production deployment.

Start with one repository and one coding agent to prove the manager loop. Add additional providers and parallel agents only after the goal-to-evidence loop works reliably.

## Research sources

See the dated research documents for the current source register and conclusions. Key primary references include [Claude Agent SDK](https://code.claude.com/docs/en/agent-sdk/overview), [Codex CLI](https://developers.openai.com/codex/cli), [Gemini CLI headless mode](https://geminicli.com/docs/cli/headless/), [OpenHands Software Agent SDK](https://github.com/OpenHands/software-agent-sdk), [LangGraph persistence](https://docs.langchain.com/oss/python/langgraph/persistence), [A2A](https://a2a-protocol.org/latest/), [MCP](https://modelcontextprotocol.io/specification/2025-11-25), and [Temporal workflows](https://docs.temporal.io/workflows).
