# ADR-0002: Define AgentRelay as a goal-oriented development manager

- **Status:** Proposed
- **Date:** 2026-10-31

## Context

The initial repository described AgentRelay primarily as a universal AI development handoff platform. The product owner's clarified intent is broader: describe a software product once, then let an AI system manage the day-to-day development process using available coding agents and tools, while the person remains responsible for goals and important decisions.

A handoff platform solves only one part of this workflow. It does not necessarily discover requirements, manage product scope, understand goal progress, replan, verify releases, or incorporate operational feedback.

## Decision

Use the following product definition:

**AgentRelay is an AI development manager that helps take software from idea to verified product outcome by managing discovery, requirements, planning, agent execution, verification, replanning, release preparation, and ongoing improvement.**

The user is the product owner. AgentRelay manages the process. Existing agents and tools perform bounded work.

Cross-agent continuity remains a foundational capability, but not the entire product.

## Design implications

1. The primary state model must represent product goals and requirements, not only tasks and conversations.
2. Task completion and requirement satisfaction must be separate states.
3. Every important completion claim must link to evidence.
4. The plan must be versioned and revisable as new facts emerge.
5. Routine work may proceed autonomously within explicit policy and budget.
6. Material product decisions and consequential external actions require human approval.
7. The execution layer must be provider-neutral but capability-aware.
8. Release and runtime feedback should eventually connect back to product goals.
9. The first milestone should be a complete vertical slice of the manager loop, even if it uses one coding agent.

## Risks

- The product scope is broad and commercially crowded.
- Current products already offer parts of the proposed workflow.
- Goal-level correctness is hard to measure.
- The system can create cost and risk if autonomy is not bounded.

## Revisit when

- User research shows that users primarily want a narrower task-handoff product.
- A competitor already provides the complete goal-to-evidence lifecycle with a materially better experience.
- The first benchmark shows no measurable advantage over a strong single-agent baseline.
