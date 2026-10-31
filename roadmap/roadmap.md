# AgentRelay Roadmap

**Status:** Proposed sequence; validate with research and experiments.  
**Last updated:** 2026-10-31

## North star

AgentRelay should become an AI development manager that can take a software goal through discovery, planning, implementation, verification, release, and improvement while keeping the human as product owner.

The roadmap is staged to prove the manager loop before expanding provider count, concurrency, or infrastructure.

## Phase 0 — Validate the problem and choose a wedge

- Interview 10–15 developers who use coding agents on real projects.
- Map their last end-to-end project workflow and manual coordination burden.
- Test whether the pain is project-level management or merely task handoff.
- Compare Factory, Shep, GitHub Copilot, Devin, CallCode, Proliferate, and app builders hands-on.
- Verify supported interfaces, authentication, terms, pricing, and local/cloud execution.
- Define a project-level benchmark and single-agent baseline.

**Exit criteria:** a specific user segment, repeated painful workflow, and measurable outcome that existing tools do not solve adequately.

## Phase 1 — Product understanding and planning experience

- Build a user-facing project workspace.
- Accept a natural-language product idea.
- Ask clarifying questions and capture unknowns.
- Generate a versioned project charter, requirements, non-goals, and success criteria.
- Generate architecture options, risks, milestones, dependencies, and acceptance checks.
- Let the user approve the initial product definition and material scope.

**Exit criteria:** a user can move from an idea to a reviewable, traceable product plan without manually writing every artifact.

## Phase 2 — Controlled goal-to-code loop

- Register one Git repository.
- Add one supported coding-agent adapter.
- Create an isolated workspace for each bounded task.
- Dispatch work from the approved plan.
- Capture diffs, commits, logs, tests, and agent outputs.
- Link evidence to requirements.
- Have AgentRelay assess gaps and select the next task.
- Add bounded retry, pause, cancellation, and human escalation.
- Present the current goal, requirements, tasks, and evidence in the workspace UI.

**Exit criteria:** a five-or-more-step dependent project chain advances without manual prompt reconstruction and can recover from at least one injected failure.

## Phase 3 — Verification and project intelligence

- Add deterministic test, lint, type, build, security, and browser checks.
- Add independent review as advisory evidence.
- Detect requirement gaps, regressions, and stale evidence.
- Version and diff plan changes.
- Add cost, latency, confidence, and intervention metrics.
- Add failure classification and retry budgets.

**Exit criteria:** the system can distinguish task completion from requirement satisfaction and explain why the project is or is not ready.

## Phase 4 — Release and operations

- Add preview environments and release-candidate management.
- Add approval-based deployment workflows.
- Record release artifacts, environment, version, and rollback plan.
- Integrate runtime errors, monitoring, and user feedback.
- Convert validated operational signals into proposed maintenance work.

**Exit criteria:** one small product can move from idea to an accepted release candidate with traceable evidence and human-controlled deployment.

## Phase 5 — Cross-provider continuity

- Add a second coding-agent adapter.
- Support switching providers while preserving project state and evidence.
- Add capability-aware task routing based on observed outcomes.
- Add session resume, provider failure fallback, and conformance tests.
- Evaluate A2A for remote agent endpoints.

**Exit criteria:** an actual task or review moves between two supported providers without manual context reconstruction or loss of provenance.

## Phase 6 — Parallelism and team workflows

- Add dependency-aware parallel tasks in separate worktrees.
- Add conflict detection and merge/rebase policies.
- Add multiple users, roles, approvals, and project-level budgets.
- Add issue tracker, CI, cloud, and collaboration integrations.
- Add team dashboards and audit exports.

**Exit criteria:** concurrent work improves throughput without unacceptable regressions, integration conflicts, or review burden.

## Phase 7 — Scale and ecosystem

- Add more adapters based on user demand.
- Add remote workspaces and hosted execution if trust and economics justify them.
- Add extension interfaces and reusable project templates.
- Evaluate business model and enterprise governance requirements.
- Publish reproducible project-level evaluation results.

## First milestone: deliberately narrow, not conceptually small

The first milestone is not a generic CLI or a two-agent demo. It is a vertical slice of the intended product:
- idea intake and clarification;
- product charter and acceptance criteria;
- dependency-aware plan;
- one coding agent;
- isolated execution;
- evidence capture;
- requirement-level verification;
- dynamic next-step selection;
- bounded recovery and human escalation;
- a simple dashboard/timeline.

One agent is enough to prove that AgentRelay manages product progress rather than just passing messages. More agents are added after the loop works.

## Explicit non-goals for the first milestone

- Every AI provider or IDE.
- A public agent marketplace.
- Unrestricted parallel agents.
- Autonomous production deployment.
- Replacing LangGraph, Temporal, A2A, MCP, or Git.
- Training a custom model or learning-based router before reliable outcome data exists.
