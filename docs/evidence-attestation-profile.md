# AgentRelay Evidence Attestation Profile

**Status:** Proposed interoperability profile; not a published standard  
**Reviewed:** 2026-11-05  
**Related:** [Product state model](product-state-model.md), [security and trust](security-and-trust.md), [ADR-0007](../decisions/ADR-0007-standardized-evidence-attestations.md)

## 1. Purpose

Define how AgentRelay should represent and verify evidence that software work was performed and that an approved product requirement is satisfied.

The objective is not to replace in-toto, SLSA, DSSE, CI systems, or software supply-chain security tools. It is to connect their evidence to AgentRelay's versioned product goals and requirements.

## 2. Evidence layers

### Layer A — Immutable subjects

Evidence must identify what it applies to. Prefer immutable digests:
- Git commit (gitCommit) and/or Git tree digest;
- binary, package, or container digest;
- immutable test configuration or workflow revision;
- environment image digest and dependency-lock digest where relevant.

A branch name, tag, PR number, agent session, or mutable URL is useful metadata but is not a sufficient subject identity by itself.

### Layer B — Standard statements and predicates

Reuse existing formats when semantics fit:
- **in-toto Statement v1:** subject plus predicate type and predicate.
- **in-toto test-result/v0.1:** test result, configuration references, and passed/warned/failed test names.
- **SLSA provenance v1:** build definition, external parameters, resolved dependencies, trusted builder identity, invocation metadata, and byproducts.
- Other vetted predicates such as SBOM, vulnerability scan, code review, and release evidence.

Do not translate an upstream signed attestation into a boolean and discard its original envelope, subject, predicate type, signer, or source URI.

### Layer C — AgentRelay requirement mapping

AgentRelay owns the product-level relationship:

GoalVersion -> RequirementVersion -> ImmutableSubject -> EvidenceSet -> PolicyEvaluation -> AcceptanceDecision

A proposed AgentRelay-specific requirement-verification predicate may include project/goal version, requirement ID/version, subjects, evidence references, result (PASS/FAIL/INCONCLUSIVE/NOT_RUN), method (deterministic/trusted_attestation/human/advisory_model), verifier identity/version, policy version, environment and dependency digests, check configuration, coverage, limitations, timestamps, superseded evidence, and invalidation reason.

The final predicate type URI must remain provisional until AgentRelay controls a stable domain and publishes a versioned schema. Do not use an unowned or placeholder URI in production.

## 3. Signing and verification

The in-toto framework recommends DSSE for the envelope. DSSE authenticates the payload and payload type, but does not define trust roots, key management, identity binding, encryption, or application policy.

A verifier must:
1. Parse and validate the envelope and Statement schema.
2. Verify the signature against an explicit trust store or identity policy.
3. Validate expected signer identity and allowed builder/workflow identity.
4. Validate predicate type and supported version.
5. Match the subject digest to the exact source/artifact being evaluated.
6. Check repository/tenant/project scope.
7. Check evidence freshness and policy version.
8. Evaluate required evidence coverage and limitations.
9. Record the decision and verifier version.

A key ID is a hint, not a trust decision. Never accept an attestation solely because a cryptographic signature is mathematically valid.

## 4. Who may issue what

| Producer | Permitted evidence | Not permitted by default |
|---|---|---|
| Coding agent | Proposed patch, logs, self-report, suggested tests, candidate evidence references | Authoritative pass/fail acceptance for its own work |
| Trusted test runner | Test-result attestation for the exact subject/configuration it executed | Claiming product-level satisfaction beyond measured checks |
| Build platform | SLSA/build provenance for its own invocation | Claims about unrelated requirements |
| Security scanner | Vulnerability/SBOM evidence for its scanned subject | Product acceptance |
| Independent reviewer agent | Advisory review findings and cited code locations | Sole authority for high-impact acceptance |
| Human product owner/reviewer | Explicit acceptance, waiver, scope change, or subjective decision | Unrecorded changes to historical evidence |
| AgentRelay verifier | Requirement-level policy evaluation over verified evidence | Inventing evidence or silently changing approved requirements |

Signing keys and workload identities must remain outside untrusted agent workspaces. For local-first use, the trust model may be user-managed; for hosted use, tenant isolation, key custody, signer identity, and auditability require a stronger service design.

## 5. Evidence freshness and invalidation

Evidence records are immutable. Current status is a derived projection.

Mark evidence stale or requiring re-verification when any relevant dependency changes:
- requirement or goal version;
- Git commit/tree or artifact digest;
- test/check definition or workflow revision;
- environment image or runtime configuration;
- dependency lock or resolved dependency set;
- acceptance policy or verifier version;
- external service state on which the check depended.

Do not invalidate unrelated evidence unnecessarily. Model dependencies between requirement, source paths/components, checks, and artifacts so invalidation can be scoped. A new passing record supersedes the prior record without deleting it.

## 6. Bundle and replay handling

An in-toto Bundle is a collection of attestations, not a signature over the complete collection. Verify every individual envelope. Detect missing, duplicated, replayed, stale, and superseded evidence. Keep an append-only index or trusted audit trail that records which evidence was considered by each acceptance decision.

For external actions and provider messages, use stable AgentRelay action IDs and idempotency keys. Reconcile the remote system after ambiguous timeouts. Do not claim exactly-once execution.

## 7. GitHub-specific availability constraint

GitHub's current documentation states that artifact attestations are available for public repositories on Free/Pro/Team plans, while private/internal repository use requires GitHub Enterprise Cloud. AgentRelay must keep its evidence store and verifier provider-neutral and support alternate/customer-managed attestation paths where necessary.

## 8. Non-goals and cautions

- This profile does not claim SLSA compliance.
- It does not prove product usefulness, user satisfaction, or complete correctness.
- It does not replace deterministic tests, security review, or human judgment.
- It does not define a new cryptographic algorithm or key-management system.
- It does not require every local development experiment to be signed.
- It does not make an LLM-as-judge result equivalent to a trusted test result.
- It does not establish a market moat; it is a proposed interoperability and trust design.

## 9. Primary references

- [in-toto Statement v1](https://github.com/in-toto/attestation/blob/main/spec/v1/statement.md)
- [in-toto test-result predicate](https://github.com/in-toto/attestation/blob/main/spec/predicates/test-result.md)
- [in-toto envelope specification](https://github.com/in-toto/attestation/blob/main/spec/v1/envelope.md)
- [in-toto Bundle specification](https://github.com/in-toto/attestation/blob/main/spec/v1/bundle.md)
- [DSSE](https://github.com/secure-systems-lab/dsse)
- [SLSA v1.2 build provenance](https://slsa.dev/spec/v1.2/build-provenance)
- [SLSA v1.2 source requirements](https://slsa.dev/spec/v1.2/source-requirements)
- [GitHub artifact attestations](https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/use-artifact-attestations)
