# AgentRelay Research Update — 2026-11-05

**Research date:** 2026-11-05  
**Repository head inspected:** 5eb30519456638b6bb14c85f39229265a8dd1520 (main; November 4 research batch)  
**Scope:** evidence interoperability; software provenance and attestations; A2A v1.0 maturity; MCP authorization; OpenTelemetry GenAI observability; retry/idempotency implications; security and evaluation changes.  
**Evidence standard:** primary specifications and official documentation preferred. Standards establish formats and protocol semantics, not that a particular implementation or claim is trustworthy. No competitor account trials, customer interviews, or production tests were performed in this run.

## Executive conclusion

The strongest new engineering direction is to make AgentRelay's evidence layer interoperable instead of inventing a proprietary format for every claim.

The in-toto Attestation Framework defines a subject/predicate statement model and a test-result predicate. SLSA v1.2 provides build and source provenance concepts. DSSE provides a recommended signing envelope for in-toto attestations. These cover much of the low-level evidence representation problem.

They do not establish that a product requirement is satisfied. AgentRelay's candidate domain-specific responsibility is to connect an approved product goal and versioned requirement to immutable source/artifact subjects, independently produced evidence, verifier policy, and an acceptance decision. This is narrower and more testable than claiming a universal evidence standard.

A2A v1.0 is described by its maintainers as stable and production-ready. MCP's reviewed HTTP authorization specification has strict resource-audience and PKCE requirements. AgentRelay should implement these semantics explicitly rather than flattening provider calls into a generic send-prompt abstraction.

## 1. Repository inspection and gaps

At the start of this run, main pointed to 5eb30519456638b6bb14c85f39229265a8dd1520. The repository already had a goal-oriented product vision, product-state model, competitor research through November 4, adapter capability profiles, event reconciliation, evidence freshness, memory integrity, security requirements, and a project-level evaluation plan.

New gaps:
1. The evidence model described digests and provenance but did not map to interoperable in-toto/SLSA predicates.
2. The system risked inventing its own test-result envelope despite an existing in-toto predicate.
3. The trust model needed to distinguish signature integrity from signer identity, builder trust, predicate semantics, and evidence quality.
4. A2A's stable v1.0 release, signed Agent Cards, protocol negotiation, and non-guaranteed SendMessage idempotency needed to be reflected.
5. MCP authorization guidance needed explicit resource-indicator and PKCE requirements.
6. Observability guidance needed the status and sensitive-content semantics of current GenAI conventions.

## 2. Reuse in-toto for evidence statements

### 2.1 Statement v1

The in-toto Statement v1 model binds a predicate to subjects identified by digest. The predicate type URI tells consumers how to interpret the claim. Subjects are assumed immutable. This is useful for binding test/build/review evidence to a precise Git commit, tree, binary, or container digest rather than a mutable branch name.

### 2.2 Existing test-result predicate

The in-toto test-result/v0.1 predicate represents:
- result: PASSED, WARNED, or FAILED;
- configuration references;
- optional test-run URL;
- passed, warned, and failed test names.

Its example binds a test result to a Git commit digest. Prefer it over a new AgentRelay-specific generic test schema when the semantics fit.

Limitation: a test-result predicate says what a test run reported. It does not prove that the tests adequately cover a product requirement or that the signer is trustworthy. AgentRelay must separately evaluate test configuration, signer, builder, subject, policy, and requirement mapping.

### 2.3 SLSA v1.2 provenance

SLSA v1.2 build provenance separates buildDefinition (build type, external parameters, internal parameters, and resolved dependencies) from runDetails (builder identity, invocation metadata, and byproducts).

The builder ID represents the trust base expected to execute the build and faithfully record provenance. Consumers should accept only expected signer/builder pairs. External parameters are inputs under tenant/user control and should be verified; resolved dependency digests help identify what the build consumed.

The SLSA Source Track distinguishes source revision history/provenance, technical controls, and two-party review. Its source-provenance examples include actor attribution, code review, authentication, vulnerability scans, test results, and SBOMs. This is a useful maturity model, not a claim that AgentRelay should seek SLSA compliance immediately.

### 2.4 DSSE and the trust gap

The in-toto framework recommends DSSE for signed envelopes. DSSE authenticates payload bytes together with the payload type and avoids depending on JSON canonicalization. However, DSSE deliberately does not define key management, identity binding, trust roots, encryption, or application verification policy. A key ID is a hint, not a trust decision.

The in-toto Bundle specification warns that the bundle as a whole is not authenticated; individual attestations must be verified. An attacker may otherwise delete valid attestations, replay obsolete ones, or insert invalid records. Preserve and verify each signed envelope, maintain a trusted append-only index/audit trail, and detect missing, duplicated, stale, or superseded evidence.

## 3. Proposed AgentRelay evidence profile

A future AgentRelay requirement-verification predicate could add:
- project ID and approved goal version;
- requirement ID and version;
- immutable subject digests;
- references/digests of supporting test, build, scan, review, and runtime attestations;
- result: PASS, FAIL, INCONCLUSIVE, or NOT_RUN;
- method: deterministic check, trusted external attestation, human decision, or advisory model review;
- verifier identity/version, policy version, environment image digest, dependency-lock digest, check/configuration digest, timestamps, coverage, and limitations;
- references to superseded evidence and reasons for invalidation.

The predicate URI must remain provisional until AgentRelay controls a stable domain and publishes a schema. Do not claim it is an established standard.

### Trust boundary

Coding agents can propose artifacts and evidence, but must not authoritatively accept their own work. A trusted verifier should run deterministic checks in a controlled environment and issue the machine-verifiable result. A separate LLM review can be advisory. Subjective requirements and consequential acceptance decisions may require an identified human.

A signature proves that a trusted signer signed particular bytes. It does not prove that a test is adequate, that the requirement was interpreted correctly, or that the product is useful. Verification policy must check signer identity, builder identity, repository/workflow, subject digest, predicate type/version, freshness, and evidence coverage.

### Evidence freshness

Keep evidence immutable. Derive current/stale/superseded/revoked state separately. Invalidate affected evidence when requirement version, source subject, environment image, dependency lock, test/check definition, or acceptance policy changes. A new commit must not inherit a previous green result by default.

## 4. A2A v1.0 is a meaningful protocol milestone

The A2A project announced v1.0 as its first stable, production-ready protocol release. The announcement describes multiple protocol bindings and version negotiation, multi-tenancy, signed Agent Cards, and polling, streaming, or webhook delivery.

The v1.0.1 specification defines task, context, history, artifact, and status semantics. Its idempotency boundary is explicit:
- Get/List operations are naturally idempotent.
- Cancel is idempotent.
- Send Message only MAY be idempotent; agents may use messageId to detect duplicates.

Implications:
- Inspect and validate Agent Cards; verify signatures when available.
- Negotiate a declared protocol version and binding.
- Persist remote task/context/message/artifact identifiers.
- Treat remote task status as execution state, not product acceptance.
- After an ambiguous timeout, reconcile remote task state before retrying a potentially non-idempotent Send Message.
- Do not claim exactly-once delivery from A2A alone.

A2A remains agent-to-agent communication, not a project-state ledger or product lifecycle manager.

## 5. MCP authorization is not just a token field

The reviewed MCP 2025-11-25 HTTP authorization specification requires clients to implement OAuth Resource Indicators and include the canonical MCP server URI in authorization and token requests. It also requires PKCE support with S256 when technically capable, secure token handling, and bearer tokens in the Authorization header—not in query strings. Servers must validate that a token was issued for their own resource and must not accept or transit tokens intended for other servers.

Implications:
- Store credentials per MCP server/resource.
- Request only necessary scopes and support step-up authorization.
- Never forward an MCP server A token to server B.
- Redact tokens, authorization codes, signed URLs, and secrets from prompts, telemetry, logs, and project memory.
- Use a distinct secret-injection model for STDIO servers; do not apply the HTTP OAuth flow blindly.

MCP connects agents to tools and context. It does not confer authorization on AgentRelay's own actions or replace its policy engine.

## 6. OpenTelemetry GenAI conventions: useful but still moving

The dedicated OpenTelemetry GenAI semantic-conventions repository marks the agent/framework conventions as Development. It describes spans for agent creation, remote/local invocation, workflows, planning, and tool execution. It warns that message content and tool arguments/results may be sensitive and are opt-in.

Recommendation:
- Pin the exact semantic-conventions revision/schema used by an exporter.
- Use standard spans where semantics match.
- Add low-cardinality agentrelay.* correlation attributes for project, requirement, work item, adapter, and evidence IDs.
- Keep prompt/tool content capture disabled by default; use explicit opt-in, redaction, retention, and access controls.
- Do not make dashboards depend on unstable experimental attribute names without a migration plan.

## 7. Durable execution and idempotency

LangGraph's durable-execution guidance recommends placing side effects in separately checkpointed tasks, isolating non-deterministic operations, and making operations idempotent so replay does not repeat side effects. A2A SendMessage semantics reinforce that a protocol may not guarantee deduplication for every operation.

AgentRelay should use stable action IDs and idempotency keys for external effects, a durable outbox/inbox or equivalent dispatch record, deduplication on provider event/message IDs where available, reconciliation after ambiguous timeouts, and an explicit unknown state rather than assuming a failed HTTP response means no effect occurred.

Do not claim exactly-once execution. Design for at-least-once delivery plus idempotent effects and reconciliation where possible.

## 8. GitHub artifact-attestation availability

GitHub's current documentation says artifact attestations are available on current plans, but Free/Pro/Team plans support them only for public repositories; private/internal repositories require GitHub Enterprise Cloud. A universal AgentRelay product cannot assume a GitHub-hosted attestation path is available for every customer.

Keep evidence storage and verification behind provider-neutral interfaces. Support customer-managed signers or other attestation systems where justified. Do not make GitHub artifact attestations the canonical product-state store.

## 9. Evaluation additions

Add tests for wrong commit subjects, untrusted signers/builders, wrong repository/workflow identity, unexpected predicate types, agent self-attestation, LLM/deterministic disagreement, stale evidence after relevant changes, bundle tampering/replay, duplicate A2A delivery, MCP token audience mismatch, telemetry leakage, and GitHub private-repository plan limits.

New metrics include valid subject-bound evidence coverage, stale-evidence detection precision/recall, invalid signer/predicate rejection, duplicate-action prevention, false acceptance rate, telemetry redaction violations, and verifier cost/latency.

Primary success remains verified product outcome with less human coordination, not the number of signed records.

## 10. Product and architecture decision

Reuse in-toto/SLSA/DSSE and established predicates for lower-level provenance and test claims. Keep the AgentRelay event ledger and requirement model canonical. Only define an AgentRelay-specific requirement-verification predicate if the product-level relationship cannot be represented cleanly with existing predicates and a controlled schema/domain is available.

This does not establish a defensible market moat. It is an interoperability and trust architecture choice. The market hypothesis remains that requirement-to-evidence lifecycle management across providers and tools reduces human coordination and improves verified outcomes; it must still be tested against Factory, Shep, GitHub, Cursor, Claude Managed Agents, and a strong single-agent baseline.

## 11. Sources reviewed (accessed 2026-11-05)

### in-toto and DSSE
- Statement v1: https://github.com/in-toto/attestation/blob/main/spec/v1/statement.md
- Test-result predicate: https://github.com/in-toto/attestation/blob/main/spec/predicates/test-result.md
- Attestation envelope: https://github.com/in-toto/attestation/blob/main/spec/v1/envelope.md
- Bundle semantics: https://github.com/in-toto/attestation/blob/main/spec/v1/bundle.md
- Validation model: https://github.com/in-toto/attestation/blob/main/docs/validation.md
- DSSE: https://github.com/secure-systems-lab/dsse

### SLSA and GitHub
- SLSA v1.2 build provenance: https://slsa.dev/spec/v1.2/build-provenance
- SLSA v1.2 source requirements: https://slsa.dev/spec/v1.2/source-requirements
- GitHub artifact attestations: https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/use-artifact-attestations

### Protocols and observability
- A2A v1.0 announcement: https://a2a-protocol.org/v1.0.1/announcing-1.0/
- A2A v1.0.1 specification: https://a2a-protocol.org/v1.0.1/specification/
- MCP 2025-11-25 authorization: https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization
- OpenTelemetry GenAI agent spans: https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-agent-spans.md
- LangGraph durable execution: https://docs.langchain.com/oss/python/langgraph/durable-execution

## Next research questions

1. Can the same requirement-verification record be consumed by GitHub Actions, Cursor, Claude, Codex, and local CI without losing signer or subject provenance?
2. Which parts of the proposed requirement predicate should be standard in-toto predicates versus AgentRelay-specific metadata?
3. What trust root is realistic for a local-first developer product versus a hosted multi-tenant product?
4. How should a verifier express partial coverage or manual acceptance?
5. Can requirement-to-evidence traceability materially reduce human coordination compared with Factory/Shep/GitHub and existing issue/CI tooling?
6. What is the smallest vertical slice that demonstrates evidence freshness, cross-provider continuity, and correct next-action selection?
