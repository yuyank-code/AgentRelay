# Initial Architecture

**Status:** Research hypothesis; not an implementation commitment  
**Last updated:** 2026-10-30

## Design goal

Separate the durable project/work model from provider-specific agent execution. AgentRelay owns the project lifecycle and evidence; coding agents own the task execution they are assigned.

## Logical components

```text
Developer UI / CLI
        |
Project & Workflow API
        |
Project State Store ---- Audit / Event Log
        |
Task Planner & Policy Engine
        |
Handoff Packet Builder
        |
Agent Adapter Interface
   |          |          |
Claude      OpenAI     Other agent
Code        coding     adapters
   |          |          |
Controlled execution workspace(s)
        |
Git diff + logs + test evidence
        |
Verification / Review Gate
        |
Human approval when required
        |
Next task / next agent
```

## Core services

### 1. Project registry
Stores project identity, repository URL/path, default branch, active workspace, project goals, constraints, and configured integrations.

### 2. Project state and evidence store
Stores structured facts:
- current branch and commit SHA;
- working-tree status and changed-file list;
- task graph and task status;
- decisions and rationale;
- commands and tests executed;
- test outcomes and logs;
- known risks, blockers, and unresolved questions;
- handoff artifacts and agent run identifiers.

Git remains the canonical source for source code. The state store is a navigable index and workflow record, not a competing source of truth.

### 3. Task planner
Converts a goal into bounded tasks. Every task should have:
- objective;
- context references;
- in-scope and out-of-scope files or components;
- dependencies;
- constraints;
- acceptance criteria;
- allowed tools/actions;
- expected evidence;
- timeout and retry policy.

### 4. Context compiler
Builds a minimal, task-specific context packet from repository metadata, docs, relevant code, prior decisions, failures, and task history. It should cite file paths, commit SHAs, and evidence IDs so the receiving agent can verify context.

### 5. Agent adapter interface
A common interface hides provider-specific invocation details while preserving capability differences. An adapter should declare:
- supported operations (plan, edit, inspect, test, review);
- input/output formats;
- session/resume behavior;
- streaming and cancellation support;
- workspace requirements;
- permission model;
- available tools;
- known limitations and version.

Do not assume all agents expose equivalent capabilities or stable APIs.

### 6. Execution manager
Runs an adapter in a bounded workspace. Prefer isolated worktrees or disposable workspaces for parallel/uncertain work. Capture stdout/stderr, exit code, duration, resource use where available, and process cancellation.

### 7. Verification and policy engine
Checks whether the task's acceptance criteria are satisfied. Start with deterministic checks (tests, lint, type checking, diff inspection) before model-based review. A model review is advisory evidence, not proof.

### 8. Handoff and workflow engine
Controls transitions such as planned → ready → running → awaiting_verification → awaiting_approval → completed/failed/blocked. LangGraph or another workflow engine may implement orchestration, but the state machine and domain contracts should remain product-owned.

## Initial data entities

- Project
- Repository
- Workspace
- Goal
- Task
- TaskDependency
- AgentProvider
- AgentAdapter
- AgentRun
- HandoffPacket
- Artifact
- Evidence
- Decision
- Approval
- Policy
- AuditEvent

## Handoff packet (illustrative)

```json
{
  "project_id": "project_123",
  "task_id": "task_042",
  "objective": "Implement role-based authorization",
  "base_commit": "abc123",
  "constraints": ["Preserve existing API contracts"],
  "relevant_files": ["src/auth/", "tests/auth/"],
  "prior_decisions": ["decision_008"],
  "known_failures": [],
  "acceptance_criteria": [
    "Unauthorized roles receive 403",
    "Existing authentication tests pass",
    "New authorization tests cover each role"
  ],
  "allowed_actions": ["read_repository", "edit_workspace", "run_tests"],
  "approval_required_for": ["network_access", "dependency_install", "push", "merge", "deploy"]
}
```

## Initial technology hypotheses

- Backend: Python + FastAPI.
- Workflow: begin with a small explicit state machine; evaluate LangGraph if branching, pause/resume, retries, and human approval justify it.
- Persistence: PostgreSQL for projects, tasks, events, and metadata.
- Repository integration: Git CLI/library with worktrees.
- Agent integration: adapter interface; evaluate official SDK/CLI/API support per provider.
- Tool integration: MCP where useful; do not confuse tool access with agent handoff.
- Agent interoperability: A2A where remote agent discovery/task semantics are useful.
- UI: defer final choice until the core workflow is validated; a CLI may be fastest for the first prototype.

## Architectural risks

1. Provider APIs and CLIs change; adapters need versioning and contract tests.
2. A conversation summary can omit critical facts; use repository and execution evidence.
3. Concurrent agents can conflict; isolate workspaces and define merge/rebase policy.
4. Model-based verification can be wrong; prioritize deterministic tests and human review.
5. Secrets may leak into prompts, logs, or artifacts; redact and scope credentials.
6. An agent may claim success despite incomplete work; require evidence and explicit state transitions.
