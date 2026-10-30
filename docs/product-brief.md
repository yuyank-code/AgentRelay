# AgentRelay — Product Brief

**Status:** Initial hypothesis, to be validated  
**Last updated:** 2026-10-30

## 1. Vision

Make software project work portable across AI models and coding environments. A developer should be able to start work with one agent, hand it to another, and continue from verified project state rather than manually copying prompts and chat histories.

## 2. Primary user

A developer or small team that uses multiple AI assistants and coding agents, and regularly switches between them for planning, implementation, debugging, review, or cost/availability reasons.

## 3. Core job to be done

"When I switch AI tools during a software project, help the next tool understand the real state of the project, what has already been tried, what remains, and exactly what it is authorized to do—so work can continue safely."

## 4. Key workflow

1. **Create/select project:** connect a local Git repository.
2. **Set goal:** record the desired outcome, constraints, and definition of done.
3. **Plan:** break the goal into small tasks with dependencies and acceptance criteria.
4. **Select executor:** choose or recommend a compatible coding agent.
5. **Prepare handoff:** build a compact, task-specific packet from project state and relevant evidence.
6. **Execute:** run the coding agent in a controlled workspace.
7. **Capture evidence:** collect diff, changed files, logs, tests, exit status, and agent summary.
8. **Verify:** run policy-approved checks and optionally ask a separate reviewer.
9. **Decide:** accept, request revision, retry, or pause for a human.
10. **Continue:** create the next task and hand it to the same or a different agent.

## 5. Product capabilities (hypotheses)

- Project registry and repository connection.
- Provider-neutral task and handoff schema.
- Coding-agent adapters with capability declarations.
- Context compiler that selects relevant project facts instead of dumping entire chat histories.
- Execution manager for local or isolated workspaces.
- Git diff and test-evidence capture.
- Verification and review gates.
- Human approval and permission policies.
- Timeline, audit log, costs, latency, and failure visibility.
- Recovery from interrupted or failed agent runs.

## 6. What AgentRelay is not

- Not a new foundation model.
- Not merely a chat UI that forwards prompts.
- Not a replacement for Git.
- Not necessarily a new general-purpose multi-agent framework.
- Not an unrestricted autonomous agent that can silently modify production systems.
- Not a promise that every commercial coding product exposes a stable automation API. Adapter feasibility must be verified individually.

## 7. Initial non-goals

- Public agent marketplace.
- Arbitrary remote agents across untrusted networks.
- Automatic production deployment.
- Autonomous purchases, account changes, or external communications.
- Supporting every IDE and coding agent in the first release.
- Replacing established workflow engines where they already solve orchestration well.

## 8. Success metrics to test

- Time required to resume a project after switching agents.
- Percentage of handoffs accepted without manual context repair.
- Task completion rate after handoff.
- Regression/defect rate compared with single-agent workflows.
- Percentage of completion claims backed by reproducible evidence.
- Human intervention frequency and reason.
- Cost and latency per successfully completed task.
- Context size/tokens required to resume work.

## 9. Key product risk

Existing coding agents and orchestration frameworks may add equivalent cross-agent continuity features. AgentRelay must demonstrate a differentiated, measurable benefit—especially repository-grounded state, robust adapter behavior, verification, and safe handoff—not simply wrap APIs.
