# Product State and Evidence Model

**Status:** Proposed domain model for research and architecture validation  
**Date:** 2026-11-02

## Purpose

AgentRelay needs a durable representation of the product being built, not just a transcript, task list, or coding-agent session. It must answer what outcome the user approved, which requirements define it, what decisions shape implementation, what work was attempted and against which repository state, what evidence supports completion, whether that evidence is stale, and what should happen next.

This is a logical model, not a requirement to adopt a graph database or RDF. PostgreSQL plus immutable event records and typed relationships may be sufficient.

## 1. Keep five kinds of information distinct

Temporal's October 2026 agent-memory/state material distinguishes context, long-term memory, execution state and effect state. AgentRelay also needs explicit product/domain state and observability.

| Information class | Meaning | Example | System of record |
|---|---|---|---|
| Product/domain state | Approved facts about the product and intended outcome | Goal, requirements, constraints, decisions, release criteria | AgentRelay |
| Execution state | Where an ongoing workflow is and what step is next | Run status, attempt count, checkpoint, waiting approval | Workflow runtime / AgentRelay event ledger |
| External effect state | Whether an action in another system actually happened | PR merged, deployment succeeded, issue created | GitHub, cloud provider, issue tracker; AgentRelay stores references and reconciles |
| Context and long-term memory | What an agent sees for its current decision and useful information retrieved from prior work | Task brief, selected architecture notes, coding conventions | Context compiler and versioned project records |
| Observability/telemetry | Measurements and traces describing what ran and how it behaved | Model latency, tool duration, token/cost estimate, retry count | Telemetry backend |

A memory summary is not proof that work completed. A workflow checkpoint is not proof that a deployment succeeded. An agent conversation is not the canonical product specification.

## 2. Core logical entities

| Entity | Required information |
|---|---|
| Project | Stable ID, owner/tenant, repository links, lifecycle status, data policy |
| ProductGoal | User-approved outcome, target users, success measures, non-goals, constraints, version, approval |
| Requirement | Stable ID, version, parent/child relation, priority, source, rationale, acceptance conditions, status |
| Assumption / OpenQuestion | Statement, source, confidence, impact if wrong, owner, resolution status |
| ArchitectureDecision | Context, options, chosen decision, consequences, author/approver, superseded decision |
| PlanVersion | Version, goal/requirement baseline, milestones, task DAG, dependencies, assumptions, budget and change rationale |
| WorkItem | Objective, linked requirements, scope, dependencies, executor capabilities, acceptance checks, retry and budget limits |
| AgentProfile | Provider/runtime/adapter versions, capabilities, auth/billing mode, workspace model, supported events and limitations |
| AgentRun | Work item, agent profile, session ID, workspace, branch/commit, status, timestamps, cost/usage, raw-event references |
| Artifact | Diff, commit, test report, log, screenshot, build, package, preview URL or release artifact; immutable digest where possible |
| Evidence | Claim evaluated, evidence type, source, commit/tree, environment, command/check, result, evaluator, timestamp, limitations and freshness |
| Release | Version/artifact, environment, deployment state, approval, health checks, rollback reference |
| OperationalSignal | User report, exception, metric alert, security finding or support issue linked to release and affected requirements |
| Approval | Requested action, scope, risk, policy decision, approver, timestamp, expiry, decision and conditions |
| AuditEvent | Append-only actor/action/input references/policy/outcome record |

Do not force every entity into the first implementation. The minimum vertical slice needs Project, ProductGoal, Requirement, PlanVersion, WorkItem, AgentRun, Artifact, Evidence, Approval and AuditEvent.

## 3. Traceability and provenance

The core chain is:

**Product goal → requirement → acceptance check → plan/task → agent run → code/artifact → verification evidence → release → operational signal → proposed improvement**

Record whether an activity used a source/artifact, generated an artifact/decision, derived one artifact from another, was informed by another activity, and which human or software agent was associated with it.

W3C PROV-O provides a useful conceptual vocabulary around Entity, Activity, Agent, usage, generation, derivation, association and plans. AgentRelay can adopt these semantics without RDF or a graph database.

Every important claim should be traceable to a specific source/version. For repository evidence, record repository identity, commit SHA, path or test artifact, and environment. For external evidence, record the provider resource ID and retrieval time.

## 4. Requirement status and evidence status are separate

Suggested requirement statuses:
- proposed;
- needs clarification;
- accepted;
- in progress;
- implemented but unverified;
- verified;
- released;
- regressed;
- superseded.

Suggested evidence outcomes:
- passed;
- failed;
- skipped;
- unavailable;
- inconclusive;
- stale.

A task can be complete while its linked requirement remains unverified. A requirement can be verified on one commit and become stale after relevant code, tests, configuration, environment or the requirement itself changes.

Evidence should support `verified` only when it refers to the current accepted requirement version, identifies the exact commit/tree or immutable artifact, identifies the environment and check version, is relevant to the acceptance condition, and has no known invalidating change. A model review is advisory evidence. A green unit-test suite is not proof of a subjective user-experience requirement.

Avoid one unqualified completion percentage. Show requirement-level status, priority, evidence freshness and unresolved uncertainty. If a roll-up is added, expose its weighting and never hide unverified or blocked critical requirements.

## 5. Plan versions are hypotheses, not truth

Each plan revision should preserve the prior plan version, triggering evidence/user input, requirements affected, tasks added/removed/reordered/split, dependency and critical-path changes, cost/risk implications, approval requirement and approver.

The manager may replan routine implementation details within policy. It must not silently change the original goal, non-goals, accepted requirements, privacy constraints or release criteria.

## 6. Events, snapshots and external systems

Use append-only domain/audit events for important state transitions, plus query-friendly current-state projections. Do not rely on a mutable Markdown summary as the only record.

Git remains authoritative for code. CI remains authoritative for checks it actually ran. GitHub remains authoritative for PR/merge state. The cloud/deployment platform remains authoritative for deployment state. AgentRelay owns the approved product goal, requirements, plan versions, approvals and cross-system links.

Before retrying an external action, reconcile the external system. Use stable idempotency keys when supported. A worker can crash after an external action succeeds but before AgentRelay records the response; blind retry can duplicate the effect.

## 7. Context compiler contract

For each task, compile a minimal view from canonical records:
- current goal and relevant accepted requirements;
- non-goals and constraints;
- applicable architecture decisions;
- dependency/task context;
- relevant source files and repository instructions;
- current branch/commit and known uncommitted changes;
- prior attempts and unresolved failures;
- expected artifacts and acceptance checks;
- allowed capabilities, permissions, time and cost budgets.

Each injected fact should carry a source reference and version. Keep full project state out of the prompt by default; retrieve details on demand. Preserve the exact compiled context or immutable manifest for audit, subject to secret/privacy filtering.

## 8. Runtime and telemetry boundary

A workflow engine may own durable execution, retries, timers, pause/resume and signals. It should not become the only product database. Temporal's event history can restore workflow execution after process failure, but product goal and external effect state still belong to their respective systems.

OpenTelemetry's GenAI conventions provide span concepts such as workflow invocation, agent invocation, tool execution and evaluation result. The current GenAI conventions are marked Development, so pin versions and avoid treating them as a stable domain schema.

Do not capture raw prompts, model outputs, tool arguments or tool results into telemetry by default. They may contain source code, user data, secrets or other sensitive information. Prefer operational metadata and references to access-controlled artifacts; allow content capture only through explicit, scoped opt-in and redaction.

## 9. Open design questions

- Which state transitions require human approval versus policy-only authorization?
- How are requirements represented for projects that begin with vague, evolving goals?
- What invalidates evidence after code, environment or test changes?
- How are multiple repositories and external systems linked to one product?
- How should users export, delete, retain or self-host project history?
- What minimum event schema supports replay/audit without storing every token?
- Which project facts should be materialized in Git and which should remain in AgentRelay's database?
- How should the manager explain uncertainty without pretending a probability score is calibrated?

## References

- [W3C PROV-O Recommendation](https://www.w3.org/TR/prov-o/)
- [Temporal: Agent memory and state](https://docs.temporal.io/ai/featured/memory-state-stripe)
- [Temporal Workflows and Event History](https://docs.temporal.io/workflows)
- [OpenTelemetry GenAI agent spans](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-agent-spans.md)
- [OpenTelemetry GenAI spans and content capture](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md)
- [OpenTelemetry GenAI events](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-events.md)

## 10. What the ledger is—and is not — 2026-11-03

GitHub Agentic Workflows now documents cross-run memory and a working-draft Git-backed Ledger: append-only JSONL records, content hashes, parent relationships, disposable SQLite projections, bounded storage, concurrent-head reconstruction, and separate trusted persistence. Therefore, "store append-only project memory in Git" is not itself a novel AgentRelay capability.

AgentRelay's product-state model must be distinguished by domain semantics and evidence relationships, not by the choice of JSONL, SQLite, PostgreSQL, or Git:
- A generic memory ledger stores records; AgentRelay must represent approved goals, versioned requirements, assumptions, decisions, acceptance checks, plan revisions, artifacts, releases, and operational signals as typed entities.
- A record's integrity hash does not authenticate its author, establish its truth, or prove the linked requirement is satisfied.
- A memory projection is a query convenience, not canonical truth.
- Agent-generated state changes are untrusted proposals. A trusted service validates and commits them.
- Evidence is valid only for the exact requirement version, commit/tree, environment, test/check version, and release to which it refers.
- A later change to a relevant dependency or artifact can invalidate prior evidence even when the original test result remains immutable.
- External systems remain authoritative for their own state; AgentRelay stores correlation IDs, snapshots, and evidence references, then reconciles before retrying actions.

The candidate differentiator is a verified semantic trace from user goal to real product outcome—not a novel storage engine or memory format.

Reference: [GitHub Agentic Workflows Git-backed Ledger Specification](https://github.github.com/gh-aw/specs/repo-memory-ledger-specification/).
