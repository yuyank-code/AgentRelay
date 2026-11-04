# ADR-0006: Keep Authorization and Product Acceptance Independent of Provider Run Status

- **Status:** Proposed
- **Date:** 2026-11-04

## Context

Provider-native agent platforms increasingly expose run states, approvals, automatic reviewers, outcome rubrics, and multi-agent workflows. Their semantics differ:

- Cursor Cloud Agents reports agent/run lifecycle and Git snapshots, with some information scoped to the agent rather than an individual run.
- Claude Managed Agents dynamic workflows can report a workflow as completed even when child threads failed; its outcome grader evaluates a bounded artifact against a rubric, not the entire product lifecycle.
- OpenAI Agents API automatic review handles supported permission requests with a provider-managed reviewer, but it is not a universal host-application authorization service; review tokens are billed separately and `review=none` does not route actions to the app for manual approval.
- Provider event streams and webhooks have different replay, delivery, and maturity guarantees.

Treating provider terminal status or provider-managed review as the source of truth would couple AgentRelay's product state and safety guarantees to each provider's implementation.

## Decision

AgentRelay owns two independent decisions:

1. **Authorization:** whether a proposed action is permitted under the user's project policy, credentials, budget, and approval requirements.
2. **Acceptance:** whether the task or product requirement is satisfied by current, reproducible evidence tied to the correct artifact and requirement version.

Provider-native permissions, approvals, and reviewers are defense-in-depth. They do not override AgentRelay policy. Provider run status is stored as execution evidence but never directly sets product acceptance to passed.

Every adapter must expose its capability gaps and event semantics. If it cannot surface an approval boundary required by policy, AgentRelay must use a stricter execution profile or refuse that action. Unknown or contradictory event state triggers reconciliation; consequential actions fail closed.

## Consequences

### Positive
- Product correctness remains independent of provider-specific status labels.
- A provider can be replaced without rewriting acceptance semantics.
- Security policy is centralized and auditable.
- Child-agent and external-side-effect failures are less likely to be hidden by a top-level completed status.

### Negative
- AgentRelay must implement and maintain its own policy and acceptance state machines.
- Event reconciliation and evidence freshness add complexity.
- Provider-native approval features may need to be duplicated or wrapped for a consistent user experience.

## Required implementation tests

- Provider says completed while a child run failed.
- Provider reviewer approves an action that AgentRelay policy denies.
- Provider stream disconnects and event history is incomplete.
- A new commit invalidates old verification evidence.
- A grader passes an artifact while deterministic checks fail.
- An external action succeeds but its acknowledgement times out.

## Sources

- Cursor Cloud Agents API: https://cursor.com/docs/cloud-agent/api/endpoints
- Cursor Automations: https://cursor.com/docs/cloud-agent/automations
- Claude Managed Agents workflow runs: https://platform.claude.com/docs/en/managed-agents/workflow-runs
- Claude Managed Agents outcomes: https://platform.claude.com/docs/en/managed-agents/define-outcomes
- Claude Managed Agents security: https://platform.claude.com/docs/en/managed-agents/secure-deployment
- OpenAI Agents API automatic review: https://developers.openai.com/api/docs/guides/agents-api/sessions/approvals
