# Evaluation Plan — Does AgentRelay Actually Manage Product Progress Better?

**Status:** Proposed experiment design  
**Last updated:** 2026-10-31

## 1. Evaluation objective

Test whether AgentRelay improves the probability and efficiency of reaching a verified product outcome across a sequence of dependent development tasks—not merely whether it can launch agents or create task lists.

The core claim to test is:

"Given the same product goal, repository, available models, time, and budget, AgentRelay helps a developer reach a more complete and reliable outcome with less manual coordination than a strong single-agent workflow or an existing orchestrator."

## 2. Baselines

Compare at least three workflows:

- **A — Single-agent baseline:** one coding agent with repository instructions and normal manual steering.
- **B — Existing product baseline:** a relevant native or third-party tool such as GitHub Copilot, Factory, Shep, or another competitor available to the tester.
- **C — AgentRelay:** goal model, versioned requirements, project evidence, dynamic planning, and policy-based escalation.

Keep model versions, repository, task specification, time budget, and token/API budget as comparable as possible. Record unavoidable differences.

## 3. Task suite

Start with one small real application and 5–10 dependent changes. Expand toward 20+ steps only after the harness is reliable.

Include:
- initial product goal with incomplete requirements;
- clarifying questions and scope trade-offs;
- architecture and database decisions;
- dependent frontend/backend implementation;
- a requirement change after implementation begins;
- a failing test or broken build;
- a task that reveals a missing dependency;
- an agent timeout or process interruption;
- a provider/session switch;
- a merge conflict or overlapping file change;
- a security-sensitive request that should require approval;
- a release-candidate and rollback decision;
- a post-release bug report or runtime signal.

A good suite must test planning, execution, evidence, and recovery—not only code generation.

## 4. Metrics

### Product outcome
- requirement coverage by independent acceptance checks;
- end-to-end user journey pass rate;
- unresolved product-critical gaps;
- release readiness and deployment correctness.

### Engineering quality
- regression count and severity;
- test coverage quality, not only line coverage;
- static-analysis and security findings;
- architectural consistency and dependency drift;
- complexity and maintainability changes.

### Manager quality
- correctness of next-action selection;
- quality of dependency ordering;
- useful re-planning after new information;
- duplicate or unnecessary tasks;
- requirement changes made without approval;
- percentage of status claims with evidence.

### Human experience
- human minutes spent writing prompts or repairing context;
- time to resume after agent/session/provider switch;
- intervention frequency and reason;
- time waiting for a human decision;
- user trust and perceived control.

### Efficiency and safety
- total cost per accepted requirement;
- wall-clock time and latency;
- number of retries and wasted runs;
- recovery success after injected failures;
- policy violations blocked;
- duplicate external side effects;
- secret leakage and cross-project data leakage.

## 5. Verification design

- Prefer deterministic tests, build results, schema checks, static analysis, browser assertions, and independent acceptance scripts.
- Use hidden or withheld acceptance checks where feasible.
- Use human review for product behavior and subjective quality.
- Use model review as supporting evidence, not as the only judge.
- Where possible, use a verifier that did not write the implementation or tests.
- Preserve raw logs, commit SHAs, tool versions, environment metadata, and evidence hashes.
- Record skipped, unavailable, failed, passed, and inconclusive checks separately.

## 6. Failure injection

Deliberately test agent process crash; workflow worker restart; provider rate limit; malformed structured output; stale or conflicting repository state; tool permission denial; network outage; repeated identical failure; plan change while work is in progress; and external action succeeds but the workflow crashes before recording the result.

The last case is important: retrying a deployment, issue creation, or other external action can duplicate side effects unless operations are idempotent or reconciled.

## 7. Benchmark context

- [SWE-bench Verified analysis by OpenAI, 2026-02-23](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/): OpenAI argues the set is increasingly contaminated and recommends SWE-bench Pro for frontier coding evaluation.
- [ProgramBench, submitted 2026-05-05](https://arxiv.org/abs/2605.03546): evaluates holistic program reconstruction rather than isolated feature/bug tasks.
- [SWE-STEPS, submitted 2026-04-03](https://arxiv.org/abs/2604.03035): studies chains of dependent pull requests and sequential software evolution.

These benchmarks inform the design but do not fully measure AgentRelay's target problem: managing an evolving product goal through discovery, planning, coding, verification, release, and feedback. AgentRelay needs its own transparent project-level task suite.

## 8. Experimental discipline

- Freeze task descriptions and acceptance checks before each run.
- Pin agent/model/runtime versions and record changes.
- Run each workflow multiple times because agent outcomes vary.
- Report failures and negative results.
- Avoid selecting only tasks that favor AgentRelay.
- Compare total cost and human effort, not only agent completion rate.
- Keep benchmark tasks and hidden checks separate where possible.
- Publish the evaluation harness and raw aggregate results if licensing and privacy permit.

## 9. Initial success gate

For the first vertical slice, require:
1. one product goal converted into versioned requirements and a plan;
2. at least five dependent implementation/verification steps;
3. no manual prompt reconstruction between steps;
4. actual evidence linked to each claimed completed requirement;
5. at least one injected failure handled through bounded recovery or clear escalation;
6. a human approval gate for a consequential action;
7. a working preview or release candidate accepted against the agreed criteria;
8. measurable reduction in human coordination time versus the single-agent baseline.

If AgentRelay cannot outperform the baseline, identify whether the failure is in goal interpretation, planning, execution, verification, recovery, or UX before adding more agents.
