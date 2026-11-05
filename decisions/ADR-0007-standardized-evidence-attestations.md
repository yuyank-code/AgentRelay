# ADR-0007: Reuse Standard Attestations; Own Requirement Acceptance Semantics

**Status:** Accepted as a research/architecture direction; implementation pending validation  
**Date:** 2026-11-05

## Context

AgentRelay needs to connect product requirements to code changes, tests, builds, reviews, releases, and operational evidence. Inventing a new envelope for every test/build claim would duplicate established software-supply-chain work and make integrations harder.

The in-toto Attestation Framework provides Statement v1 and predicates including test-result. SLSA v1.2 provides build and source provenance models. DSSE is the recommended in-toto signing envelope. These formats do not, by themselves, express whether a user-approved product requirement is satisfied.

## Decision

1. Reuse in-toto Statement v1, existing predicates, SLSA provenance, and DSSE where their semantics fit.
2. Preserve original signed envelopes, subject digests, predicate types, signer identities, and source references.
3. Keep AgentRelay's product-state ledger and requirement acceptance policy canonical.
4. Define an AgentRelay-specific requirement-verification predicate only if needed to connect a goal/requirement version to immutable subjects and verified evidence.
5. Keep the custom predicate URI provisional until AgentRelay controls a stable domain and publishes a versioned schema.
6. Do not let a coding agent authoritatively sign acceptance of its own work. Trusted runners, external attesters, and humans have distinct issuer roles.
7. Verify signer identity, builder identity, repository/workflow, subject digest, predicate type, freshness, and evidence coverage. A valid signature alone is insufficient.
8. Keep evidence immutable and derive stale/superseded/revoked status separately.

## Consequences

### Positive
- Reuses existing formats and tooling for test/build provenance.
- Improves interoperability with CI, GitHub attestations, supply-chain tools, and future verification systems.
- Keeps the potentially distinctive product semantics focused on goal/requirement-to-evidence traceability.
- Makes evidence source and trust assumptions inspectable.

### Costs and risks
- in-toto, SLSA, and DSSE require verifier, signer, and trust-policy integration; they are not turnkey truth systems.
- Some predicates and tooling may still evolve.
- GitHub artifact-attestation availability differs by repository visibility and plan.
- A custom requirement predicate requires schema governance, versioning, and compatibility work.
- Cryptographic provenance does not prove that tests are sufficient or the product meets user needs.

## Rejected alternatives

- **Invent a complete proprietary attestation envelope:** duplicates existing formats and weakens interoperability.
- **Treat test status or agent self-report as requirement acceptance:** conflates execution with product correctness.
- **Require all evidence to be signed immediately:** adds friction before the trust model and use case justify it.
- **Make GitHub artifact attestations the canonical store:** creates a provider/plan dependency and does not solve product-state semantics.

## Revisit when

- hands-on validation shows users need a simpler evidence representation;
- the in-toto predicate ecosystem changes materially;
- AgentRelay has a controlled domain and concrete consumers for a custom predicate;
- a trust model for local-first versus hosted use is selected;
- benchmark results show requirement-level attestations improve acceptance accuracy or auditability.
