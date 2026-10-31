# AgentRelay — Product Brief

**Status:** Strategic hypothesis; validation required  
**Last updated:** 2026-10-31

## 1. Vision

AgentRelay is an AI development manager that helps move a software product from an initial idea through discovery, requirements, architecture, implementation, verification, release, and ongoing improvement.

The user defines the intended outcome, constraints, preferences, and approval boundaries. AgentRelay manages the day-to-day development process by maintaining product context, planning work, coordinating existing AI agents and tools, checking evidence, replanning when needed, and escalating decisions that require a person.

This is broader than transferring work between AI coding sessions. Cross-agent continuity is one enabling capability inside a goal-oriented development system.

## 2. Primary user hypothesis

Initial target: solo developers and very small teams who use one or more AI coding agents to build products and find themselves manually acting as product manager, technical lead, project coordinator, reviewer, and context carrier.

This is a hypothesis, not validated customer research. The first user is also the product's own builder, which makes personal workflow observation useful but insufficient for market validation.

## 3. Core job to be done

"When I describe a software outcome, help me turn it into a working, verifiable product and keep development moving without requiring me to manually plan and prompt every next step. Keep me informed and let me make the important decisions."

## 4. User and system responsibilities

### The user owns
- the product goal and intended users;
- business, ethical, and product trade-offs;
- constraints, budget, and acceptable risk;
- approval of material scope changes and consequential actions;
- final acceptance of product behavior and release decisions.

### AgentRelay owns
- clarifying questions and discovery artifacts;
- maintaining the current product and engineering model;
- creating and revising plans and task dependencies;
- choosing a suitable execution capability within policy;
- dispatching work and capturing results;
- assessing progress against the original goal;
- attempting bounded recovery and replanning;
- communicating blockers and asking for human decisions.

### Coding agents and tools do
- research or implementation tasks assigned to them;
- repository inspection and code changes;
- tests, builds, browser checks, and other delegated work;
- structured reports and artifacts through supported interfaces.

AgentRelay coordinates these capabilities; it does not need to recreate every model, coding agent, test runner, or deployment system.

## 5. Intended lifecycle

1. **Discover:** understand the problem, target users, existing constraints, and unknowns.
2. **Define:** create a project charter, requirements, user journeys, non-functional requirements, and definitions of success.
3. **Design:** develop architecture, data models, interfaces, technical decisions, and risk analysis.
4. **Plan:** produce milestones, workstreams, dependencies, acceptance checks, and budgets.
5. **Build:** dispatch bounded work to compatible coding agents in controlled workspaces.
6. **Verify:** run deterministic checks, inspect diffs, review behavior, and map evidence to requirements.
7. **Replan:** update tasks and priorities when tests fail, assumptions change, or dependencies are discovered.
8. **Release:** prepare release candidates and deployment plans; require policy-based approval for production changes.
9. **Operate:** eventually observe errors, user feedback, and operational metrics.
10. **Improve:** turn validated feedback into maintenance and product-development work.

Not every project needs every stage. The lifecycle should be configurable rather than a rigid checklist.

## 6. Product capabilities

- Project registry and versioned product charter.
- Clarifying-question and discovery workflow.
- Requirements and acceptance-criteria model.
- Architecture and decision records with rationale.
- Dependency-aware roadmap and task graph.
- Goal-progress assessment and dynamic replanning.
- Provider-neutral, capability-aware agent adapters.
- Repository, filesystem, issue tracker, CI, browser, and deployment integrations.
- Context compiler with source paths, commit SHAs, and evidence references.
- Isolated execution workspaces and resource budgets.
- Test, build, lint, type-check, security, and browser evidence capture.
- Independent review as advisory evidence, not a sole oracle.
- Failure detection, bounded retries, checkpoints, and recovery.
- Risk-based human approval and escalation.
- Project timeline, status, blockers, cost, latency, and audit history.
- Post-release monitoring and maintenance feedback in later phases.

## 7. What success means

Success is not the number of agents connected or tasks marked complete. It is whether AgentRelay helps a user reach a better verified product outcome with less coordination burden and acceptable cost and risk.

Candidate measures:
- percentage of stated requirements satisfied by independent checks;
- time from idea to accepted preview or release candidate;
- human minutes spent writing next-step prompts and repairing context;
- successful continuation after agent/session/provider changes;
- number and severity of regressions;
- percentage of status claims backed by reproducible evidence;
- quality of re-planning after injected failures or requirement changes;
- cost and latency per accepted requirement;
- human intervention frequency, time, and reason;
- unauthorized actions blocked and false approvals avoided.

## 8. Product boundaries and non-goals

- Not a foundation model or general agent framework.
- Not a replacement for Git, CI, issue trackers, cloud providers, or coding agents.
- Not unrestricted production autonomy.
- Not universal support for every IDE or provider in the first release.
- Not a requirement to use multiple agents for every task.
- Not a guarantee that vendor consumer subscriptions can be embedded in third-party products.

## 9. Key risks

1. Existing products already cover substantial parts of this vision.
2. Broad autonomy can create cascading errors and hidden costs.
3. Product requirements may be vague or internally contradictory.
4. Model-generated tests and model-generated reviews can share blind spots.
5. Provider APIs, CLIs, permissions, and terms can change.
6. Agent state is not the same as durable project state or external transaction state.
7. Product-level completion is harder to prove than task-level test success.
8. The project could become an expensive wrapper without a clear differentiated workflow.

## 10. Validation questions

- When did developers last lose time moving work between agents or sessions?
- What did they have to reconstruct manually?
- Which decisions do they still want to make themselves?
- Which current products already solve the pain well enough?
- Would they trust an AI manager to replan and continue without approval for routine changes?
- What evidence would make them trust a completion claim?
- Would they pay for continuity, verified outcomes, governance, or convenience—and which one is the actual purchase driver?
