# AgentRelay Research Program

**Started:** 2026-10-30  
**Status:** Active

## Research questions

1. Is cross-agent project continuity a sufficiently painful and underserved problem?
2. Which parts are already solved by coding agents, IDEs, orchestration frameworks, A2A, and MCP?
3. What is the smallest differentiated product layer worth building?
4. Which coding environments can be integrated reliably through supported interfaces?
5. How should project state be represented so that handoffs are reproducible and provider-neutral?
6. How can AgentRelay verify work rather than trusting agent self-reports?
7. What security model makes autonomous execution safe enough for real repositories?
8. What evaluation demonstrates that handoffs improve outcomes?

## Research standards

- Prefer primary documentation, specifications, SDK references, release notes, and source repositories.
- Record access/review dates and distinguish documented facts from assumptions.
- Verify whether a capability is supported by an official interface, a community wrapper, or fragile UI automation.
- Compare end-to-end workflows, not just feature checklists.
- Record negative findings and alternatives rejected.
- Do not treat a model-generated answer as independent verification.
- Avoid copying proprietary documentation or long source passages.

## Current documents

- [Competitive landscape](competitive-landscape.md)
- [Initial architecture](../architecture/initial-architecture.md)
- [Product brief](../docs/product-brief.md)
- [Security and trust](../docs/security-and-trust.md)
- [Roadmap](../roadmap/roadmap.md)
- [ADR-0001](../decisions/ADR-0001-product-layer.md)

## First research hypothesis

The most promising product wedge is not generic agent-to-agent messaging. It is **verified development continuity across coding environments**: project state capture, context compilation, execution evidence, and policy-controlled handoff.

This hypothesis must be tested against current coding-agent products and emerging interoperability tools before committing to implementation.
