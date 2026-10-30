# Security and Trust Model

**Status:** Initial requirements; threat modeling required before execution features ship.

## Assets to protect

- Source code and intellectual property.
- API keys, OAuth tokens, SSH keys, and environment secrets.
- Local files outside the selected repository.
- Git history and remote repositories.
- Production credentials and deployment systems.
- User instructions, project context, and audit records.

## Threats

- Prompt injection in repository files, issues, dependency docs, or tool output.
- Agent executing destructive shell commands.
- Secret exfiltration through prompts, logs, network calls, or generated artifacts.
- Unauthorized pushes, merges, releases, or deployments.
- Malicious or compromised adapters.
- Cross-project context leakage.
- Dependency or build-script compromise.
- Concurrent agents overwriting each other's work.
- False completion reports or fabricated test results.

## Initial controls

1. Run agents with least-privilege filesystem and process permissions.
2. Use isolated worktrees or disposable sandboxes for execution.
3. Never place broad personal credentials in model context.
4. Redact secrets from prompts, logs, diffs, and artifacts.
5. Require explicit approval for push, merge, deploy, destructive commands, credential access, and external side effects.
6. Make network access deny-by-default or policy-controlled.
7. Record the actor, adapter/version, task, workspace, commit, changed files, commands, results, and approvals.
8. Treat repository content and tool output as untrusted data, never as higher-priority instructions.
9. Enforce project and tenant boundaries on all context retrieval.
10. Provide cancellation, timeouts, resource limits, and rollback/recovery paths.

## Approval tiers (proposal)

- **Tier 0 — Read-only:** inspect repository, read docs, summarize.
- **Tier 1 — Workspace edits:** modify isolated working tree; no push or external side effects.
- **Tier 2 — Local execution:** run approved tests/build commands in a sandbox.
- **Tier 3 — Network/dependency access:** requires explicit policy and potentially per-run approval.
- **Tier 4 — External/destructive actions:** push, merge, deploy, delete, publish, or modify external systems; explicit human approval required.

## Open questions

- Which sandbox model works across macOS, Linux, and cloud execution?
- How should secrets be injected without exposing them to the model?
- How can policy be enforced when a provider CLI has its own permission model?
- What audit retention and data deletion guarantees are required?
- How do we safely handle untrusted repository instructions and generated scripts?
