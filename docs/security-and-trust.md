# Security and Trust Model

**Status:** Initial requirements; threat modeling required before execution features ship.  
**Last updated:** 2026-10-31

## Security objective

AgentRelay coordinates agents that can read source code, edit files, run commands, access networks, use credentials, and eventually affect deployment systems. Security must be enforced by the runtime and policy layer, not left to model instructions.

## Assets to protect

- Source code, intellectual property, and Git history.
- API keys, OAuth tokens, SSH keys, signing keys, and environment secrets.
- Local files outside the selected project.
- Product requirements, user data, project decisions, and audit records.
- Databases, CI/CD systems, cloud accounts, and production environments.
- Other projects and tenants.

## Threats

- Prompt injection in repository files, issues, websites, dependency docs, or tool output.
- Agent executing destructive shell commands or exfiltrating data.
- Secrets leaking through prompts, logs, diffs, screenshots, or artifacts.
- Unauthorized pushes, merges, releases, deployments, or external communications.
- Malicious or compromised adapters and MCP servers.
- Cross-project or cross-tenant context leakage.
- Dependency/build-script compromise.
- Concurrent agents overwriting each other's work.
- False completion reports, fabricated tests, or stale evidence.
- Repeated retries causing cost runaway or duplicate external side effects.
- A model silently changing the user's product goal or requirements.

## Initial controls

1. Run agents with least-privilege filesystem, process, and network access.
2. Use isolated worktrees or disposable sandboxes for uncertain execution.
3. Never place broad personal credentials in model context.
4. Inject scoped credentials at runtime and redact secrets from logs and artifacts.
5. Require explicit approval for push, merge, production deployment, destructive operations, credential access, and external side effects.
6. Make network access deny-by-default or policy-controlled.
7. Treat repository content and tool output as untrusted data, not higher-priority instructions.
8. Record actor, adapter/version, task, workspace, commit, commands, evidence, policy decisions, and approvals.
9. Enforce project and tenant boundaries on every context retrieval.
10. Provide cancellation, timeouts, resource limits, spend caps, and rollback/recovery paths.
11. Use idempotency keys or external-state reconciliation before retrying operations with side effects.
12. Version product requirements and never silently alter the user's original goal.
13. Separate the model's recommendation from the policy engine's authorization decision.
14. Verify evidence against the correct repository commit and environment.

## Approval tiers (proposal)

- **Tier 0 — Read-only:** inspect project, repository, docs, logs, and metrics.
- **Tier 1 — Isolated workspace edits:** modify a disposable or dedicated worktree.
- **Tier 2 — Local execution:** run approved tests, builds, and static checks in a sandbox.
- **Tier 3 — External resources:** network access, dependency installation, external APIs, cloud test environments, and sensitive data access require explicit policy and possibly per-run approval.
- **Tier 4 — Consequential actions:** push, merge, release, production deployment, destructive commands, data migrations, credential changes, publish, delete, or external communications require explicit human approval by default.

## Product-level trust controls

- The user approves the product charter, material scope changes, and release criteria.
- AgentRelay shows what evidence supports a completion status.
- An implementation agent cannot be the only verifier of its own work.
- AgentRelay explains why a task is blocked and what decision is needed.
- Every plan revision is versioned and shows changed requirements, dependencies, and expected consequences.
- Budget and retry policies stop runaway autonomous loops.
- The user can pause, cancel, inspect, or roll back supported work.

## Protocol and provider considerations

MCP's specification emphasizes user consent, privacy, and tool safety, while noting that protocol-level support does not replace implementation authorization. AgentRelay must enforce its own policies even when an agent supports MCP or provider-specific permission hooks.

Anthropic's current Agent SDK documentation states that third-party products must not offer Claude.ai login or subscription rate limits unless approved; product integration should use the documented API-key path. See [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview).

## Open questions

- Which sandbox model works across macOS, Linux, Windows, and cloud execution?
- How should credentials be injected without exposing them to the model?
- Can a provider's own permission model be combined safely with AgentRelay's policy?
- What happens if an external action succeeds but AgentRelay crashes before recording it?
- What audit retention, export, and deletion guarantees are required?
- How are untrusted project instructions distinguished from user-approved policy?
- How should production monitoring and runtime data be filtered before entering model context?
