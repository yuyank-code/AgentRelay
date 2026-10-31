# Product Vision — AgentRelay

**Last updated:** 2026-10-31  
**Status:** North-star hypothesis, not a delivery promise

## The user's intended experience

A person should be able to explain the software product they want in ordinary language, provide important constraints, and then stop acting as the messenger between AI tools.

AgentRelay should take responsibility for keeping the project moving. It should discover what is unclear, develop a product and technical plan, assign implementation work to available coding agents, inspect the actual results, decide what should happen next, and repeat the cycle. The user should be able to monitor progress and intervene when a decision requires human judgment.

The user is the product owner. AgentRelay is the development manager. Existing coding agents and tools are the workforce.

## Example

A user says:

"I want to build a crowd-management platform for large public events. It should manage cameras, detect crowd density, identify dangerous congestion, and help staff respond."

AgentRelay should not simply convert this sentence into one coding prompt.

It should:
1. clarify who uses the system, where it runs, what data it may process, and what success looks like;
2. research the domain and identify technical, privacy, and operational constraints;
3. produce a product brief, user journeys, requirements, architecture options, and open questions;
4. identify dependencies and prepare a staged development plan;
5. send implementation work to suitable coding agents and tools;
6. observe code changes, tests, logs, and preview behavior;
7. determine whether the implementation satisfies the intended requirement;
8. change the plan when an assumption proves wrong or a dependency is missing;
9. request human approval for material scope, privacy, security, cost, or deployment decisions;
10. continue until the agreed acceptance conditions are met, then support maintenance and improvement.

The exact agent and tool choices are implementation details. The durable responsibility is advancing the product goal with traceable evidence.

## The manager loop

### 1. Understand
Maintain a current model of the product goal, users, requirements, constraints, assumptions, risks, and success measures.

### 2. Assess
Compare the current project state with the intended outcome. Identify satisfied requirements, missing evidence, blockers, regressions, and newly discovered work.

### 3. Decide
Propose the next best action: research, clarify, design, implement, test, review, fix, release, monitor, or escalate.

### 4. Authorize
Check the action against budget, scope, dependency, permission, and risk policies. The model may recommend an action; the policy engine decides whether it is allowed automatically or requires approval.

### 5. Execute
Dispatch the bounded action to a compatible agent or tool in a controlled workspace.

### 6. Verify
Capture reproducible evidence and assess whether the action produced the expected result.

### 7. Update and continue
Update the product model and event history, then reassess the goal. Do not treat a successful task as proof that the whole product is complete.

## What autonomy means

Autonomy should be graduated, not binary.

- **Routine:** AgentRelay can automatically inspect, plan, run approved tests, and make bounded isolated changes.
- **Material:** AgentRelay should explain and request approval for requirement changes, architecture trade-offs, new external services, sensitive data handling, and meaningful budget changes.
- **Consequential:** Production deployment, credential access, destructive operations, data migrations, merges to protected branches, and external side effects require explicit approval unless a narrowly scoped policy says otherwise.
- **Unresolved:** repeated failures, conflicting evidence, ambiguous requirements, or low confidence should trigger escalation with a concise explanation and recommended options.

The goal is to reduce routine human coordination, not remove human accountability.

## What would make this meaningfully different

A prompt router sends instructions. A task manager tracks work. A coding agent implements a task. AgentRelay's proposed product-level responsibility is to maintain a traceable relationship:

**Original goal → requirement → design decision → work item → code change → verification evidence → release → operational feedback**

This relationship should let AgentRelay answer:
- What did the user actually ask for?
- Which requirements are satisfied, partially satisfied, or unverified?
- What evidence supports that status?
- Which decisions changed the plan and why?
- What is the next action that most advances the goal?
- What is blocked, risky, or waiting for a human?
- Did the released product behave as expected?

This is a hypothesis for differentiation, not proof of a market gap. Current competitors already offer substantial agent orchestration, memory, model routing, approval, and end-to-end coding features. See the [competitive landscape](../research/competitive-landscape.md).

## Non-negotiable product rules

- Never equate an agent's final message with verified completion.
- Never silently change the user's product goal.
- Preserve the source and version of requirements, decisions, and evidence.
- Do not retry indefinitely or spend without a budget.
- Do not allow the agent that made a change to be the only source of its verification.
- Do not use unsupported UI automation when a stable CLI, SDK, API, or protocol exists.
- Do not assume subscription authentication is permitted for third-party orchestration.
- Keep the user informed of meaningful decisions, not every low-level tool call.
