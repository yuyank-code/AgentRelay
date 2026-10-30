# AgentRelay Roadmap

**Status:** Proposed sequence; validate with research and experiments.

## Phase 0 — Validate the problem
- Interview developers who switch between coding agents.
- Map current manual handoff workflows.
- Compare existing products and wrappers.
- Verify official APIs/CLIs and legal/licensing constraints.
- Define measurable baseline and success criteria.

**Exit criteria:** A clearly evidenced problem and a narrow first user workflow.

## Phase 1 — Local project continuity prototype
- Register one local Git repository.
- Capture commit, branch, status, changed files, project instructions, and recent task history.
- Define provider-neutral Task and HandoffPacket schemas.
- Generate a compact handoff packet for a human to inspect and copy.
- Store task/event history locally.

**Exit criteria:** A developer can resume work with a different agent using a reproducible packet.

## Phase 2 — First coding-agent adapter
- Select one coding agent with a supported programmatic/headless interface.
- Execute a bounded task in an isolated worktree.
- Capture output, exit status, diff, and test evidence.
- Add timeout, cancellation, and failure states.

**Exit criteria:** One task can be dispatched and its result reliably captured.

## Phase 3 — Cross-agent handoff
- Add a second provider/agent adapter.
- Convert first agent's execution evidence into a second agent's task context.
- Support review, revision, and acceptance flows.
- Add conflict detection and safe workspace handling.

**Exit criteria:** Demonstrate a real project task moving from agent A to agent B without manual reconstruction of context.

## Phase 4 — Verification and policy
- Add deterministic test/lint/type-check gates.
- Add reviewer agent as advisory evidence.
- Add approval tiers and audit log.
- Add secret redaction and sandbox policy.

**Exit criteria:** Workflow cannot silently promote unverified or unauthorized changes.

## Phase 5 — Developer experience
- CLI first, then dashboard if validated.
- Project timeline, agent status, diff and test evidence.
- Pause/resume, retry, cancel, and manual override.
- Cost and latency tracking.

## Phase 6 — Scale and interoperability
- Evaluate A2A for remote agent discovery and task interoperability.
- Evaluate MCP for tool access.
- Add more adapters based on user demand.
- Support remote workspaces and team projects only after local workflow is reliable.

## Explicit non-goals for MVP
- Every AI provider.
- Autonomous production deployments.
- Marketplace or agent social network.
- General-purpose orchestration framework replacement.
