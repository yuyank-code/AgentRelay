# Competitive Landscape

**Reviewed:** 2026-11-01  
**Confidence:** Desk research from public product pages and official documentation. Feature claims are vendor-stated unless otherwise noted; hands-on validation and customer interviews remain outstanding.

## Executive conclusion

The original differentiation hypothesis—provider-neutral handoffs, coding-agent adapters, project memory, approvals, and multi-agent routing—is not defensible as a feature list. Those capabilities are already present in meaningful combinations across GitHub Copilot, Factory, Shep, CallCode, Proliferate, and other products.

The expanded idea—an AI development manager that owns the goal-to-evidence lifecycle from product discovery through release and ongoing improvement—is a more useful product framing, but it is not an empty market. Factory and ServiceNow make broad software-factory or outcome-driven claims; GitHub and specialist tools already cover substantial execution and governance steps.

The only credible path is to identify a measurable user outcome or workflow that current products do not handle well, then prove it with a benchmark and user research.

## 1. Direct and adjacent competitors

| Product | Publicly described overlap | Competitive implication | What still needs validation |
|---|---|---|---|
| GitHub Copilot cloud agent and third-party agents | Assign an issue or prompt, run asynchronous coding work, create pull requests, iterate through review comments; GitHub documents third-party Claude and Codex agents in public preview. | Strong distribution and repository-native workflow. A user may not need a separate router for common issue-to-PR tasks. | Cross-provider project-level goal state, product discovery, multi-repository lifecycle, and long-horizon goal-to-runtime traceability. |
| Factory Droids / Factory | Plan, build, review, and ship; model choice across vendors; codebase understanding; persistent knowledge; adjustable autonomy; mission control, analytics, and automations. | Very direct competitor to a model-independent development manager. Its public positioning already overlaps with much of the proposed vision. | Hands-on depth, exact lifecycle coverage, evidence semantics, pricing, portability, and customer experience. |
| Shep | Open-source, local-first coding-agent orchestrator; many CLIs; separate Git worktrees; optional spec/research/plan approval; CI, review, pull requests, and human triage. | Directly commoditizes multi-agent worktrees, provider choice, task pipelines, and approval gates. | Whether it manages product-level goals and long-term requirement-to-release feedback, beyond feature/PR workflows. |
| CallCode | One request becomes parallel tasks; coordinates Claude Code, Codex, Cursor, Copilot, and Gemini CLI; promotes use of existing subscriptions. | Direct overlap with multi-agent orchestration and subscription-backed local agents. | Long-term product memory, verification depth, and release/operations lifecycle. |
| Proliferate | Public positioning describes parallel coding agents, isolated workspaces, review gates, MCP integrations, and recurring/event-driven automations. | Direct overlap with agent workspaces and engineering workflow orchestration. | Product maturity, availability, exact supported adapters, and lifecycle coverage; public site was not fully inspectable during this scan. |
| CodeMerlin | Positions itself around engineering governance from specification and planning through implementation and review, with GitHub/GitLab/Jira/Linear and coding-agent integrations. | Overlaps with spec-driven delivery and governance. | Hands-on product behavior, exact automation boundaries, and evidence/verification details. |
| Specto | Positions itself as an AI product architect that turns an idea into PRD, technical specification, architecture, and sprint plan. | Shows that idea-to-plan and AI product-management stages are also becoming product categories. | Whether it executes code and manages delivery, rather than focusing on planning artifacts. |
| Devin | Autonomous software-engineering agent with task execution, code/test workflows, pull requests, and review-oriented capabilities. | Strong alternative to building a general coding workforce manager if one autonomous agent can complete the user's tasks. | Multi-provider control, product-goal memory, and how much human steering is needed in real projects. |
| Bolt | Turns natural-language app descriptions into full-stack web apps and offers preview and deployment. | Competes for simple idea-to-app use cases by removing the need for an external manager. | Generality beyond its managed app-building environment and portability across coding agents. |
| ServiceNow AI Workflow Factory / Autonomous Engineer | Enterprise positioning around business outcomes, autonomous planning/building/testing, third-party agents/tools, and governance through an AI control tower. | Strong adjacent signal that outcome-oriented, governed agentic execution is a strategic direction for large platforms. | Its target is enterprise workflow transformation, not necessarily an independent developer's cross-provider software project. |
| OpenHands | SDK and Agent Server for building and running coding agents in local or ephemeral workspaces. | More of an enabling engine/runtime than a direct product competitor, but it can reduce the need to build an execution runtime. | Fit with AgentRelay's desired control plane and integration model. |

## 2. What is already commoditized or weakly differentiated

| Proposed capability | Current assessment |
|---|---|
| Handoff packets and prompt/context transfer | Weak as a standalone differentiator. Agents already support project instructions, sessions, memory, and repository context. |
| Agent routing and model selection | Weak. Factory, GitHub, and specialist orchestrators expose model or agent selection. |
| Parallel coding agents and Git worktrees | Weak. Shep and other tools already provide this workflow. |
| Task breakdown and dependency orchestration | Weak. Frameworks and commercial products provide task graphs, plans, crews, and workflows. |
| Persistent state and session resume | Weak by itself. LangGraph, AutoGen, CrewAI, Temporal, agent SDKs, and products provide variants. |
| Human approval and audit logs | Necessary but not unique. GitHub, agent frameworks, and enterprise products already expose controls. |
| Tests, PRs, CI, and code review | Necessary but not unique. Native platforms and agent products increasingly handle these. |
| Provider-neutral adapters | Useful infrastructure, but adapters are copyable and expensive to maintain. |
| Goal-to-evidence traceability across discovery, requirements, design, code, tests, release, and runtime feedback | Potential wedge, but not proven unique. Factory and ServiceNow claim broader outcome-oriented loops; more competitor research and hands-on tests are required. |
| Honest measurement of long-horizon project progress and recovery | Potential research/product wedge. Could become an advantage if AgentRelay demonstrates better outcomes with public, reproducible evaluations. |

## 3. Potential positioning to test

A possible position is:

**An AI development manager for solo builders and small teams that keeps the original product goal, project decisions, and verification evidence connected while existing agents do the work.**

This is more specific than "universal agent orchestrator," but it is still only a positioning hypothesis. It must be tested against Factory for broad model-independent software-factory workflows; Shep for local-first, multi-agent coding workflows; GitHub Copilot for repository-native agent delegation; Devin for autonomous task execution; and app builders such as Bolt for simple idea-to-deployed-app workflows.

## 4. Possible defensibility—not yet established

Potential sources of durable value:
1. A high-quality, versioned product-state and requirement/evidence model that remains portable across vendors.
2. Deep integrations into a user's repositories, CI, issue tracker, preview environments, monitoring, and release process.
3. A benchmark and evaluation dataset for sequential product evolution, re-planning, recovery, and requirement coverage.
4. Measured routing and planning policies based on actual task outcomes, cost, latency, and defect rates.
5. Strong trust, security, local execution, and governance that developers can understand and audit.
6. A developer experience that makes the system feel like a responsible product/technical lead rather than a collection of agent sessions.

None of these is automatically a moat. They become defensible only through execution quality, user trust, integration depth, proprietary workflow learning, or ecosystem adoption.

## 5. User validation still required

No customer interviews have been conducted in this research run. Interview 10–15 developers who use coding agents regularly. Ask about their last real project, not whether they like the concept:
- What did they manually coordinate?
- What context was lost?
- Which steps still needed their judgment?
- Which current product did they try?
- What failed or felt unsafe?
- What would they pay to stop doing?
- What would make them trust a system to continue without them?

Do not treat interest in an ambitious vision as proof of willingness to adopt or pay.

## 6. Sources reviewed on 2026-10-31

- [GitHub Copilot third-party coding agents](https://docs.github.com/en/copilot/concepts/agent-capabilities/about-agents/about-third-party-coding-agents)
- [GitHub Copilot on GitHub.com and Mobile](https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/copilot-surfaces/copilot-on-github)
- [Factory](https://factory.com/)
- [Factory Droids](https://factory.com/product/droids)
- [Shep](https://shep.bot/)
- [CallCode](https://callcode.dev/)
- [Proliferate](https://www.proliferate.dev/)
- [CodeMerlin](https://www.codemerlin.ai/)
- [Specto](https://specto.codes/)
- [Devin](https://devin.ai/)
- [Bolt AI app builder](https://bolt.new/use-cases/ai-app-builder)
- [ServiceNow AI Workflow Factory announcement, 2026-10-06](https://newsroom.servicenow.com/press-releases/details/2026/ServiceNow-launches-AI-Workflow-Factory-to-turn-workflow-improvement-into-a-continuous-agentic-AI-powered-loop/default.aspx)
- [OpenHands Software Agent SDK](https://github.com/OpenHands/software-agent-sdk)

## 7. Open gaps

- Hands-on feature and pricing comparison for Factory, Shep, GitHub, Devin, CallCode, and Proliferate.
- Confirm whether product-level goal and requirement traceability is genuinely differentiated.
- Test whether users prefer an independent manager or a single strong coding agent with good project instructions.
- Assess whether the first target should be solo builders, small teams, or enterprise engineering groups.


## 8. Competitive refresh — 2026-11-01

This refresh deepens the comparison using current official product and documentation pages. These remain vendor-stated capabilities; no hands-on product test was performed in this run.

### Shep is a closer competitor than a basic parallel-agent runner

The current [Shep product page](https://shep.bot/) describes a workflow that starts from a short feature description and can optionally generate research, requirements, and an approved YAML plan before implementation. It then runs one or more selected coding CLIs in separate Git worktrees, watches CI, can push bounded fixes after CI failures (the page describes up to three configurable retries), routes agent questions to a human with a recommended answer, and stores per-step checkpoints that can be diffed or restored. It also provides web, CLI, and terminal interfaces.

**Implication:** Shep already covers a meaningful portion of the feature-level spec → implement → review → PR loop, including agent choice, approval, retries, and checkpoints. AgentRelay must not claim these capabilities as novel.

The public Shep pages reviewed here do not establish whether it maintains a long-lived product model spanning discovery, changing requirements, multiple repositories, releases, production signals, and a traceable goal-to-outcome ledger. That is a research question, not a confirmed absence.

Sources: [Shep product page](https://shep.bot/), [Shep documentation](https://shep.bot/docs).

### GitHub is becoming both a multi-agent surface and an agentic workflow runtime

GitHub documents third-party Claude and Codex coding agents in public preview. Users can assign agents from repository Agents tabs, issues, pull requests, mobile, or VS Code; agents create reviewable pull requests and can iterate from review comments. The page describes repository context and GitHub policy controls.

Separately, [GitHub Agentic Workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows) lets developers define repository automations in Markdown and run them through GitHub Actions using Copilot, Claude, Codex, or Gemini. The documentation describes read-only defaults, declared safe outputs, secrets isolated from the agent runtime, firewalled execution, threat detection, and run-level usage/cost inspection.

**Implication:** a user whose pain is simply “run Claude/Codex/Gemini on repository tasks with guardrails” may already have a native GitHub route. AgentRelay's opportunity, if any, must be broader than repo automation: a durable product-level control plane that coordinates work across the user's full software lifecycle and potentially across systems outside GitHub.

Sources: [GitHub third-party coding agents](https://docs.github.com/en/copilot/concepts/agent-capabilities/about-agents/about-third-party-coding-agents), [GitHub Agentic Workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows).

### Factory's public positioning directly overlaps with the expanded vision

Factory's current Droids page claims cross-model routing, codebase understanding, adjustable autonomy, consistent memory across tools, and an end-to-end prompt-to-PR workflow covering planning, implementation, testing, and pull request creation.

**Implication:** AgentRelay cannot position itself merely as “one AI manager for multiple models.” It needs a specific user segment and measurable workflow advantage, or deeper product-level state and lifecycle integration that Factory's actual product does not provide. This requires hands-on validation.

Source: [Factory Droids](https://factory.com/product/droids).

### OpenHands provides more than a coding SDK

The OpenHands Software Agent SDK provides Python, TypeScript, and REST interfaces for coding agents and local or ephemeral workspaces. A separate [OpenHands Automation Service](https://github.com/OpenHands/automation) owns cron schedules, webhooks, dispatch, sandbox lifecycle, and run history; its repository describes the service as beta.

**Implication:** AgentRelay can reuse an existing execution and scheduling stack if it fits, rather than building every runtime primitive. OpenHands still does not automatically supply AgentRelay's chosen product-goal and requirement/evidence model.

Sources: [OpenHands Software Agent SDK](https://github.com/OpenHands/software-agent-sdk), [OpenHands Automation Service](https://github.com/OpenHands/automation).

### Codex is explicitly positioned as an embeddable harness

OpenAI's 2026-08-19 article describes the Codex CLI, SDK, and app-server as reusable agent infrastructure. App-server exposes threads, turns, events, interruption, tools, and approval requests. The article's architecture assigns the host application responsibility for its own interface, domain context, tools, and operational boundaries.

**Implication:** building a second generic coding-agent harness is unnecessary for the Codex path and likely wasteful elsewhere. AgentRelay should own the manager and product state, then integrate native harnesses through adapters.

Source: [Codex as a platform](https://developers.openai.com/blog/codex-as-a-platform).

## 9. Revised differentiation test

| Candidate claim | Current evidence | Verdict |
|---|---|---|
| Universal model/agent routing | Factory, Shep, GitHub, and other products already advertise multiple providers or agents. | Not defensible alone. |
| Handoff and persistent agent context | Existing SDKs and products provide sessions, memory, repository instructions, and continuity. | Table stakes. |
| Spec and plan generation before code | Shep and product-planning tools already advertise this. | Not unique. |
| Worktree isolation, retries, CI repair, approvals | Shep and native platforms cover substantial parts. | Not unique. |
| Natural-language repository automation with safe outputs | GitHub Agentic Workflows covers this. | Not unique for repo-scoped automations. |
| Goal-to-requirement-to-code-to-test-to-release-to-runtime traceability across tools | No complete independent proof found in this desk scan; Factory and enterprise platforms make broad adjacent claims. | Candidate wedge; unproven. |
| Reliable autonomous replanning across a long-lived product with evidence and bounded authority | Public product pages make claims, but comparative independent evidence is lacking. | Research and benchmark opportunity, not a moat yet. |

A stronger positioning hypothesis to test is:

**AgentRelay is a product-level control plane for solo builders and small teams: it maintains the user's intended product outcome and its evidence across planning, coding agents, repositories, releases, and feedback systems, then decides and coordinates the next justified action.**

This still may be beaten by an incumbent. Validate it through hands-on trials and interviews before investing in a large implementation.


## 9. Competitive refresh — 2026-11-02: GitHub workflows, Copilot Memory, and Jules

This review adds current official documentation. These are vendor-documented capabilities, not hands-on performance findings.

### GitHub Copilot dynamic workflows

GitHub documents dynamic workflows as programs combining deterministic code with one or more agents. They can run sequentially or in parallel, split a goal into tasks, pass structured results, have subagents verify findings, request user input, pause at checkpoints, and resume paused or limit-stopped runs. Limits can cover concurrent/total agents, runtime and approximate AI credits. The feature is marked public preview.

Important boundary: sharing a workflow extension shares its definition, not the original run history or saved progress. A paused run can reuse saved results from completed steps when resumed, but that is not a durable product record spanning separate milestones, providers and releases.

**Implication:** generic orchestration, structured handoffs, subagent review, budgets, pause/resume and progress views are not defensible differentiators.

### GitHub Agentic Workflows and Copilot Memory

GitHub Agentic Workflows define repository automations in Markdown and run them through Actions. Current docs list Copilot, Claude, Codex and Gemini engines. Security controls include read-only defaults, declared safe outputs, secrets isolated in downstream jobs, firewalled execution and threat detection. Cost combines Actions minutes and model inference; AI Credit estimates are best-effort.

Copilot Memory stores repository facts and user preferences and can be used across Copilot surfaces including cloud agent, CLI, code review, app, Slack and Teams. However, repository facts are scoped to one repository, memory use is a model judgment rather than a guaranteed rule, and the documented default expiry is 90 days without reinforcement.

**Implication:** persistent memory and cross-surface context are not unique by themselves. AgentRelay's potential difference must be explicit, portable product state: versioned requirements, decisions, plan changes, evidence freshness and external outcome references across providers.

### Google Jules

Jules documents plan-before-code, user review/approval, mid-task feedback, plan revision, activity feeds, diffs, pause, and retry of some transient failures. Its REST API is alpha and exposes Source, Session and Activity resources; sessions can be created against GitHub repositories, polled, messaged, and optionally configured to create a PR. API-created sessions auto-approve plans unless `requirePlanApproval=true` is supplied.

**Implication:** Jules is a credible coding-agent adapter candidate and a stronger competitor than a simple CLI runner. Its API alpha status and approval defaults need explicit handling. Public docs reviewed here do not establish a cross-provider product goal/evidence ledger; this is an open comparison question, not a confirmed absence.

### Updated overlap assessment

| Capability | Assessment |
|---|---|
| Natural-language task intake and plan-before-code | Available in Jules and other coding products |
| Multi-stage workflows, parallel agents and subagent verification | Available in GitHub Copilot dynamic workflows |
| Multi-engine repository automations | Available in GitHub Agentic Workflows |
| Cross-surface repository/user memory | Available within Copilot's ecosystem |
| Pause, resume, approval, retries and progress feeds | Available in multiple products |
| Provider-neutral goal/requirement/evidence model across releases and external systems | Potential wedge; not validated |
| Evidence freshness and requirement-level release readiness | Potential wedge; hands-on tests required |
| Runtime feedback linked to original requirements | Potential wedge; investigate further |

Do not infer that a competitor lacks a capability because its public documentation does not describe it. Test the product directly.

## 10. User-workflow signals — 2026-11-02

A recent r/ClaudeCode discussion includes a commenter who describes themselves as a non-developer building a complex business website. Their workflow uses a master design document, Claude to refine and oversee a ticket system, scheduled Codex work to implement tickets, Claude to review, and human testing/bug reports to drive the next cycle. Other comments describe wiki/meeting-note ingestion, research/review loops, deterministic gates, separate worktrees and independent diff review.

This resembles the intended AgentRelay experience, but also shows that users can approximate it manually with existing tools. It is a qualitative lead, not market validation: the discussion is self-selected and does not establish prevalence or willingness to pay.

Sources:
- [r/ClaudeCode: orchestrating multiple coding agents](https://www.reddit.com/r/ClaudeCode/comments/1x3a7ni/how_do_you_guys_orchestrate_multiple_coding/)
- [r/ClaudeCode: community discussion of workflows](https://www.reddit.com/r/ClaudeCode/comments/1tkjy4u/claude_code_dropped_workflows/)

## 11. Updated differentiation hypothesis

The strongest testable hypothesis is a **provider-neutral product-state and evidence layer** that preserves the user's approved outcome and connects requirements, decisions, plans, code changes, verification evidence, releases and operational feedback across tools.

It should answer which approved requirement a task serves; what code changed and against which commit; which check ran in which environment; whether evidence is still current; what remains unverified; why the plan changed; whether a PR/merge/deployment actually happened; and what should happen next.

This is not yet a moat. It becomes valuable only if users prefer the workflow and AgentRelay measurably reduces coordination effort or improves outcomes.

## Sources reviewed on 2026-11-02

- [GitHub Copilot dynamic workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/dynamic-workflows)
- [Using dynamic workflows](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/use-dynamic-workflows)
- [GitHub Agentic Workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows)
- [GitHub Copilot Memory](https://docs.github.com/en/copilot/concepts/prompting-and-context/copilot-memory)
- [Jules getting started](https://jules.google/docs/)
- [Jules running tasks](https://jules.google/docs/running-tasks/)
- [Jules plan review](https://jules.google/docs/review-plan/)
- [Jules errors and failures](https://jules.google/docs/errors/)
- [Jules REST API](https://jules.google/docs/api/reference/)
- [W3C PROV-O](https://www.w3.org/TR/prov-o/)
- [Temporal agent memory and state](https://docs.temporal.io/ai/featured/memory-state-stripe)
- [Temporal Workflows](https://docs.temporal.io/workflows)
- [OpenTelemetry GenAI agent spans](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-agent-spans.md)
- [OpenTelemetry GenAI spans](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md)

## 12. Competitive stress test — 2026-11-03

**Conclusion:** the proposed feature-level differentiation is weaker than previously assessed. Current public documentation for Factory, Shep, and GitHub Agentic Workflows overlaps with most of AgentRelay's stated capabilities. This is desk research, not a hands-on product test.

### Factory is the strongest broad product-level competitor

Factory's current Software Factory positioning covers enterprise intent and codebase context through planning, building, validation, release, continuous signals, model routing, mission control, persistent agent compute, analytics, and integrations with engineering systems. Its Missions product describes end-goal decomposition into dependent subtasks, resource allocation, parallel Droids, progress coordination, multi-repository migrations, and multi-PR feature delivery. These are vendor-described capabilities; public marketing does not independently establish reliability or customer outcomes.

Sources: [Factory Software Factory](https://factory.com/), [Factory Missions](https://factory.com/product/missions), [Factory IDE](https://factory.com/product/ide).

### Shep is a strong local-first individual-developer baseline

The current Shep site identifies version 1.253 and describes an MIT-licensed, local-first orchestrator. Its published flow includes starting an app from an idea, optional spec generation with requirements/research/plan, multiple coding-agent CLIs and model providers, a worktree per feature, per-step checkpoints, approval questions, CI monitoring, automatic fixes with configurable retries, circuit breakers, and human-controlled merge. It says local state is held in SQLite under the user's home directory.

This substantially overlaps local-first, bring-your-own-agent, context continuity, spec, approval, recovery, and developer-experience claims. The exact depth of its product-wide requirement traceability and release/runtime feedback should be tested hands-on rather than inferred from what its public page does not mention.

Source: [Shep product documentation](https://shep.bot/).

### GitHub Agentic Workflows now covers several patterns central to AgentRelay

The official documentation describes:
- **Dynamic Workflows:** deterministic program-defined stages combined with agent reasoning, sequential/parallel work, structured handoffs, subagent verification, user checkpoints, monitoring, pause/cancel/resume, and approximate agent/credit/time limits. Reusing a workflow definition does not share its run history or saved progress.
- **Feature Grower:** a recurring loop that reassesses a long-lived feature against its plan, current repository state, closed child issues, and advisory memory, then creates only the next useful implementation increment. An open-child gate supplies backpressure.
- **ResearchPlanAssignOps:** research findings become issues, a planner turns them into sub-issues with acceptance criteria, coding agents implement them, and a human reviews/merges. The documented pattern keeps human checkpoints at each transition.
- **ProjectOps:** AI-assisted project-board summaries, routing, and field updates. The documentation explicitly says maintainers remain responsible for planning and prioritization.
- **MemoryOps and Git-backed Ledger:** cache, repository-branch and comment memory, plus a working-draft append-only JSONL ledger with disposable projections and separate trusted persistence.
- **Security architecture:** isolated agent jobs, scoped permissions, buffered safe outputs, separate trusted jobs, network controls, and threat detection.

Sources: [Dynamic workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/dynamic-workflows), [Feature Grower](https://github.github.com/gh-aw/patterns/feature-grower/), [ResearchPlanAssignOps](https://github.github.com/gh-aw/patterns/research-plan-assign-ops/), [ProjectOps](https://github.github.com/gh-aw/patterns/project-ops/), [MemoryOps](https://github.github.com/gh-aw/patterns/memory-ops/), [Git-backed Ledger](https://github.github.com/gh-aw/specs/repo-memory-ledger-specification/), [Security architecture](https://github.github.com/gh-aw/introduction/architecture/).

### Updated overlap matrix

| Proposed AgentRelay differentiator | Current competitive evidence | Assessment |
|---|---|---|
| Multi-provider coding-agent adapters | Shep, Factory, GitHub Agentic Workflows | Weak alone |
| Persistent context/memory | Factory, Shep local state, Copilot Memory, GitHub MemoryOps/repo ledger | Weak; generic memory is commoditized |
| Spec/requirements and task planning | Shep spec mode, Factory Missions, GitHub ResearchPlanAssignOps/Feature Grower | Weak to moderate; product-wide semantics need testing |
| Parallel work and isolated workspaces | Shep worktrees, Factory Missions, GitHub agent workflows | Weak |
| CI-driven recovery and checkpoints | Shep, provider harnesses, GitHub workflows | Weak |
| Human approval and policy controls | Shep, GitHub safe outputs, Codex/Claude controls, Factory governance | Weak unless enforcement is independently stronger and cross-system |
| Product goal-to-requirement-to-release/runtime evidence graph | Pieces exist across Factory, GitHub Projects/Actions, CI and telemetry; no unified comparative test completed | Candidate, not proven |
| Cross-provider, cross-code-host, cross-lifecycle continuity under user control | Plausible; Factory is broad and GitHub is deep within its ecosystem | Candidate, not proven |
| Simpler manager UX for solo builders/non-specialist product owners | Not established by public docs; Shep and Factory already offer idea-to-work flows | Candidate, requires interviews |

### Positioning correction

Do not claim AgentRelay is unique because it is agent-agnostic, persistent, evidence-based, safe, or autonomous. These are table stakes or are directly claimed by competitors.

The only plausible wedge worth testing is a **user-owned, provider- and tool-neutral product-goal control plane** that maintains semantically typed requirements, decisions, acceptance evidence, releases, and operational feedback across more than one code host and execution system, while presenting a low-friction experience to an individual builder or small team.

Factory's published Software Factory positioning already approaches this broad lifecycle. GitHub's patterns cover much of the repository-centered loop. This wedge is not defensible until a hands-on benchmark demonstrates an advantage in product-level traceability, cross-system portability, human coordination effort, or total cost.

Do not infer competitor feature absence from public documentation. Record untested items as unknown.

### Evidence quality and remaining gaps

Vendor pages are primary evidence of vendor claims, not independent proof of quality or adoption. GitHub docs are primary evidence of documented workflow semantics and preview limitations. The 2026-09-23 empirical study of 1,248 GitHub Agentic Workflow files across 276 repositories found that 9.4% explicitly addressed prompt-injection defense; this is a sample-specific workflow-authoring result, not a universal prevalence rate. No user interviews, paid conversion tests, or hands-on competitor trials have yet been completed.

Source: [Specifying and Maintaining Agentic Workflows: An Empirical Study](https://arxiv.org/abs/2609.27263), 2026-09-23.
