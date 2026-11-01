# Initial Architecture — Goal-Oriented Development Manager

**Status:** Research hypothesis; not an implementation commitment  
**Last updated:** 2026-11-01

## Design goal

Separate AgentRelay's durable product/project management responsibilities from provider-specific agent execution.

AgentRelay owns the product model, goals, requirements, decisions, plans, policies, evidence, and lifecycle. Coding agents and tools perform bounded work. A workflow runtime makes long-running processes resumable. A policy layer controls what may happen without a human.

## Two-plane architecture

### Control plane — AgentRelay's product intelligence

- Project and product-goal registry.
- Discovery and requirements model.
- Product/engineering knowledge graph.
- Architecture and decision records.
- Roadmap, dependency graph, and task planner.
- Goal-progress assessor and dynamic replanner.
- Context compiler with provenance.
- Agent capability registry and task router.
- Verification policy and acceptance engine.
- Approval, escalation, budget, and audit services.
- Project timeline and user-facing dashboard.

### Execution plane — agents and developer tools

- Coding-agent adapters.
- Research and documentation tools.
- Git repositories and isolated worktrees.
- Local or remote execution workspaces.
- Test/build/lint/type-check/security tools.
- Browser and end-to-end testing.
- CI, preview deployment, and release tooling.
- Observability and issue/feedback integrations.

The flow is: User/product owner → AgentRelay project workspace → goal, requirements, decisions and state → assess goal gap → plan → policy check and approval if needed → agent/tool router → coding agent or tool → isolated workspace, Git and tests → diffs, logs and evidence → requirement verification → update state, replan and continue → release/operate/improve with approval gates.

## The central manager loop

1. **Observe:** load the latest project model, repository state, open tasks, agent runs, and evidence.
2. **Assess:** compare current evidence with the goal and requirements.
3. **Plan:** choose whether the next action is discovery, clarification, design, implementation, verification, recovery, release, or escalation.
4. **Authorize:** apply deterministic policies for scope, budget, credentials, network, and external side effects.
5. **Execute:** dispatch a bounded action to a suitable adapter.
6. **Verify:** inspect actual results and run policy-approved checks.
7. **Commit state:** append events, link evidence, update requirement status, and version plan changes.
8. **Continue or escalate:** choose the next action, or pause for a human.

The LLM proposes and reasons. The policy engine authorizes. The workflow engine persists and schedules. Execution tools enforce workspace and permission boundaries.

## Durable product model

Do not store the whole project as one ever-growing prompt or chat transcript. Use versioned, typed records with links to their sources.

### Core entities

- **Project:** identity, repositories, environments, owner, budget, lifecycle status.
- **Goal:** intended outcome, target users, success measures, constraints, non-goals.
- **Requirement:** stable ID, version, priority, rationale, status, acceptance criteria.
- **Assumption:** claim that may be wrong, source, confidence, validation action.
- **Decision:** decision, alternatives, rationale, author/approver, timestamp, affected requirements.
- **ArchitectureComponent:** module/service/data boundary and relationships.
- **Workstream / Milestone:** product or engineering outcome and dependencies.
- **Task:** objective, scope, dependencies, acceptance checks, budget, allowed actions.
- **AgentCapabilityProfile:** provider, runtime, supported operations, auth mode, version.
- **AgentRun:** adapter, model, workspace, inputs, session ID, start/end, status, cost, usage.
- **Artifact:** document, patch, build, test report, screenshot, deployment, or other output.
- **Evidence:** source, content hash, timestamp, commit SHA, environment, command, result, linked requirements.
- **Release:** version, environment, approvals, artifacts, rollout and rollback details.
- **RuntimeSignal / Incident:** error, metric, user feedback, source, severity, affected requirements.
- **Approval:** requested action, risk, context, decision, approver, expiration.
- **AuditEvent:** append-only record of actor, action, input references, outcome, and policy decision.

Git remains the source of truth for code. External systems remain the source of truth for their own state. AgentRelay stores a navigable, versioned project record and evidence links rather than trying to replace those systems.

## Goal and requirement traceability

The product's most important relationship is:

Goal → requirement → acceptance check → task → agent run → commit/diff → test/review evidence → release → runtime signal.

Requirement status should be one of:
- proposed;
- needs clarification;
- accepted;
- in progress;
- implemented but unverified;
- verified;
- released;
- regressed;
- superseded.

A task can be complete while a requirement remains unverified. A release can exist while the overall product goal remains only partially satisfied.

## Planning and re-planning

The planner should produce a dependency-aware graph, not just a flat list. It should consider requirement priority and user value; technical dependencies and critical path; uncertainty and need for research; testability and expected evidence; risk, budget, latency, and agent capability; current failures and blocked tasks; and changes to requirements or external constraints.

Plan revisions must be versioned and explain what changed and why. The model should not silently rewrite the original goal. Material changes require approval.

## Context compiler

Build task-specific context from current goal and relevant requirements; architecture and decision records; relevant repository files and instructions; Git branch, commit, diff, and status; task dependencies and prior attempts; known failures and evidence; allowed actions and expected output schema.

Each fact should include provenance: source path or system, commit/version, timestamp, and confidence where appropriate. Prefer targeted retrieval over dumping the entire repository or conversation history into the prompt.

## Agent adapter contract

Do not normalize all providers into a lowest-common-denominator text prompt. Each adapter should publish a capability profile:

- task execution and supported task types;
- local, hosted, or remote workspace model;
- file and shell access;
- structured output and event streaming;
- session resume/fork behavior;
- follow-up input and cancellation;
- permission hooks and approval callbacks;
- network controls and credential handling;
- usage/cost reporting;
- version and compatibility constraints.

The router should choose only among agents that satisfy the task's requirements and policy. Model-quality routing should begin with transparent rules; learn from outcome data only after enough reliable observations exist.

## Verification and evidence

Use a layered verifier:
1. repository and diff integrity;
2. deterministic unit/integration tests;
3. lint, formatting, type checking, and build;
4. static analysis, dependency and secret scanning;
5. browser or end-to-end checks for user-visible behavior;
6. independent code review as advisory evidence;
7. human acceptance for subjective or consequential outcomes.

An LLM reviewer is not an independent oracle if it shares the same assumptions and test design as the implementation agent. Preserve raw evidence and distinguish passed, failed, skipped, unavailable, and inconclusive checks.

## Workflow runtime options

- **Explicit state machine:** simplest starting point; enough for the first vertical slice if runs are short and local.
- **LangGraph:** useful for stateful reasoning graphs, checkpoints, human interrupts, replay/time-travel debugging, and recovery inside agentic decision loops.
- **Temporal:** useful when project workflows must survive long outages, run for days/weeks, receive external signals, and reliably resume from event history.
- **CrewAI / Microsoft Agent Framework / Google ADK:** alternatives for agent/team/workflow composition, but not substitutes for AgentRelay's product domain model.
- **A2A:** use when communicating with remote agents that expose the protocol.
- **MCP:** use to connect agents to tools and project data.

Do not adopt multiple workflow frameworks at once. Select one after a small experiment establishes requirements for pause/resume, idempotency, retries, external signals, and observability.

## Persistence and infrastructure hypotheses

- PostgreSQL for product state, requirements, tasks, events, approvals, and metadata.
- Object storage for large logs, test reports, screenshots, and other artifacts.
- Git and worktrees for code snapshots and isolated changes.
- Local runtime or containerized workspaces for agent execution.
- A durable event log for workflow state changes and audit.
- FastAPI/Python is a plausible backend starting point, not a locked decision.
- A web dashboard is the main product experience; CLI commands can support developers and debugging.
- Add queues, Redis, vector search, or Kubernetes only when measured needs justify them.

## Deployment and operations

A mature version may manage preview environments, release candidates, deployment plans, and operational feedback. Production rollout, database migrations, credential changes, destructive actions, and rollback decisions need explicit policies and human approvals. Runtime signals should create proposed work, not silently change the product goal.

## Architectural risks

1. A model can misunderstand the original goal or invent requirements.
2. A task graph can look complete while user-facing requirements remain unmet.
3. Evidence can be incomplete, stale, fabricated, or tied to the wrong commit/environment.
4. Provider APIs, CLI behavior, and auth models change.
5. Workflow retries can repeat external side effects unless actions are idempotent.
6. Parallel agents can conflict or create integration debt.
7. Prompt injection can arrive through repositories, issues, websites, or tool output.
8. Verification may share the implementation agent's blind spots.
9. Cost and latency can grow non-linearly in open-ended loops.


## AgentRelay manager versus provider agent harness

**Design update, 2026-11-01**

AgentRelay should not rebuild the generic coding-agent loop when a supported provider or runtime already supplies one.

### Responsibility boundary

| AgentRelay owns | Provider harness/runtime owns |
|---|---|
| Original product goal and approved scope | Model-specific reasoning and tool loop |
| Users, requirements, assumptions, success measures | Provider context compaction and session internals |
| Architecture decisions, roadmap, milestones, dependencies | Native file, shell, browser, and MCP tool execution |
| Cross-agent and cross-session project continuity | Provider-specific conversation/session IDs and resume mechanics |
| Task selection and capability-aware routing | Model selection and provider-native subagents |
| Cross-system evidence and requirement status | Raw provider events, tool outputs, and execution logs |
| Product-level risk, budget, approval, and escalation policy | Runtime sandbox and permission hooks, subject to AgentRelay's outer policy |
| Release readiness and later operational feedback | Provider-specific execution, streaming, and interruption |

The provider harness is not automatically trusted to determine that the product goal is satisfied. It reports execution facts; AgentRelay evaluates those facts against the approved product model and acceptance policy.

### Adapter contract: preserve differences

Adapters should expose a capability profile and map only common lifecycle concepts into AgentRelay's event model. Do not reduce every provider to a single prompt/response interface.

Suggested normalized events:
- run_created and run_started;
- progress_updated;
- tool_or_action_requested;
- approval_requested;
- artifact_created;
- checkpoint_created;
- usage_reported;
- run_completed, run_failed, run_cancelled, or run_interrupted.

Each normalized event should retain a reference to the raw provider event and artifact. Unsupported events must be marked unsupported, not silently simulated.

### Manager loop and safety boundary

The LLM manager may recommend the next action, but a deterministic policy layer must authorize it. A durable workflow runtime records state transitions and reconciles external side effects. The execution runtime enforces workspace, filesystem, process, and network boundaries.

The manager should be able to choose among research, clarification, architecture, implementation, testing, review, recovery, release preparation, monitoring, and escalation. The available executor can be a coding agent, research agent, deterministic tool, CI job, or external integration; not every step needs a coding model.

Do not treat an agent's internal task list as AgentRelay's authoritative product plan. Store the product plan separately, version revisions, and record why each revision occurred.
