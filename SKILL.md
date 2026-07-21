---
name: change-discovery
description: Classify and clarify proposed code changes before implementation, especially when the user cannot judge scope or a request may affect architecture, user flows, data, APIs, dependencies, security, performance, deployment, or multiple components. Determine whether the change is small, medium, or major; give a preliminary product-value assessment and improvement direction; stop on decline or defer before deeper work; recommend an advisory low, medium, or high model/reasoning tier without switching models; pause medium or major proceed/reduce-scope work for model confirmation before deeper discovery; research similar GitHub work before comparing solutions; produce a requirements brief and approval-gated plan; then apply relevant engineering disciplines using capabilities available in the host agent.
license: MIT
metadata:
  author: kongleiwork-art
  version: "1.3"
---

# Change Discovery

## Purpose

Help a non-technical user decide what is worth building and turn an uncertain request into clear requirements and a safe engineering plan. Optimize for correct total effort and low rework, not for the fewest discovery tokens.

## Host Compatibility

Treat this workflow as host-independent. Use the current agent's native tools for project inspection, public research, questions, planning, and approval. Adapt tool and skill names instead of assuming a particular vendor, model family, filesystem layout, or interaction UI.

Preserve the safety boundaries even when the host lacks a dedicated read-only, planning, or approval mode. State capability limitations instead of pretending that a tool, skill, model tier, or research action is available. Ignore host-specific metadata files that the current agent does not recognize.

## Hard Gate

Decide the change size yourself; never ask the user to classify it. Before the user explicitly approves a medium or major change, restrict work to read-only project inspection, public research, questions, requirements, options, and an in-chat plan. Do not edit project files, write code, create implementation artifacts, install dependencies, commit, push, or make external write actions.

For a small change, state the classification, a proportionate product assessment, and the model-tier recommendation, then continue with the current selection by default to avoid an extra confirmation round. Say explicitly that the work is continuing because the change is small. Respect a user's standing request to pause even for small changes.

For a medium or major change, complete a preliminary product assessment before any deeper discovery, using only the project evidence already available from initial inspection. Then apply the product-decision gate:

- **decline** or **defer** — stop for a product decision. Do not recommend a model tier for deeper discovery, research GitHub, interview deeply, write full requirements, compare implementation options, plan, or implement until the user explicitly chooses to continue anyway or revises the request.
- **reduce scope** — recommend the reduced or improved direction, recommend the model tier for that revised scope, and stop at the model checkpoint.
- **proceed** — recommend the model tier for the assessed scope and stop at the model checkpoint.

Give one direct next action. For the model checkpoint, ask the user to reply **continue** to keep the current model/reasoning selection, or to adjust it first and then reply **continue**. For a decline or defer stop, ask the user to reply **continue anyway** only if they still want discovery, or to revise/stop. This product or model checkpoint is separate from the later approval of the implementation plan.

If later evidence upgrades a small change to medium or major, or changes the product verdict, stop before further discovery or implementation, report the reclassification or revised assessment, and re-enter the matching gate. If scope is uncertain, use the higher provisional class until one material answer resolves it.

## First Response Budget

For medium or major work, the pre-gate reply must stay compact and decision-oriented. Include only:

1. **Classification** — size, 2–4 concrete reasons or consequences, confidence.
2. **Preliminary Product Assessment** — short verdict with evidence gap, smallest alternative, and one improvement direction.
3. **Next Gate** — either the product decision stop (`decline` / `defer`) or the model-tier recommendation plus model checkpoint (`proceed` / `reduce scope`).

Do not start GitHub research, long interviews, requirements drafts, option matrices, or implementation plans before the relevant gate clears. Prefer roughly one short screen of text over an essay. Load [references/first-response.md](references/first-response.md) when an example shape helps.

## 1. Classify the Change

Inspect enough code, documentation, configuration, tests, and recent history to understand current behavior and blast radius. Classify the request as small, medium, or major and report the reasons, concrete user or operational consequences, and confidence. Do not use changed line or file count as the sole measure.

Classify as **major** when any high-risk trigger applies:

- production data, schema, migration, deletion, or difficult recovery;
- public API, protocol, persisted format, or backward compatibility;
- authentication, authorization, security, privacy, compliance, or payments;
- architecture, core framework, runtime, deployment, or critical dependency;
- multiple services, platforms, or both frontend and backend;
- a core user journey or behavior affecting many existing users;
- concurrency, caching, persistence, or critical performance behavior;
- staged rollout, migration, monitoring, or a dedicated rollback plan;
- an irreversible decision or a failure that could materially affect production.

Also classify as major when two or more breadth signals combine, such as multiple modules, a new external integration, code plus deployment changes, several test layers, a background or asynchronous workflow, or effects on existing consumers.

Classify as **medium** when behavior changes within one bounded component, the audience and dependencies are limited, and rollback is straightforward. Use a compact version of the full discovery workflow.

Classify as **small** only when the change is local, reversible, preserves external contracts and data, adds no meaningful dependency, permission, deployment, or operational risk, and has a narrow verification path. Examples include copy, styling, documentation, or a well-isolated correction.

Translate technical risk into practical consequences. Say, for example, “older clients may stop signing in,” not only “backward-compatibility risk.”

## 2. Preliminary Product Assessment

Before the product or model gate, give every medium or major request a concise product assessment based on current project evidence. For a small request, include a proportionate version without adding ceremony. State:

- the problem, affected user, and intended outcome as understood;
- evidence that the problem exists, or the assumption still needing validation;
- the likely product and user value, plus the cost of doing nothing;
- risks to user behavior, trust, accessibility, or the product's existing positioning;
- the smallest viable option, including non-code or lower-risk alternatives;
- at least one concrete improvement or reduced-scope direction.

Recommend **proceed**, **reduce scope**, **defer**, or **decline**. Label this conclusion **preliminary** and say which evidence or GitHub research could change it.

Do not rubber-stamp the request. Prefer **reduce scope**, **defer**, or **decline** when evidence is weak, the user problem is unclear, the expected value is low relative to maintenance cost, or a non-code fix is enough. Choose **proceed** only when project evidence makes the user problem and likely value clear enough to justify deeper discovery. Do not turn the improvement direction into a full implementation-option comparison before research.

### Product Decision Gate

After the preliminary verdict:

- If **decline** or **defer**, stop. Explain why in plain language, name the missing evidence or better timing, and offer the strongest smaller alternative if one exists. Do not open the model checkpoint unless the user explicitly chooses to continue discovery anyway.
- If **reduce scope**, treat the reduced or improved direction as the working request for the rest of the workflow unless the user explicitly insists on the original scope.
- If **proceed**, continue to the model-tier recommendation and model checkpoint.

After a cleared gate, deepen or revise the assessment using GitHub findings and material answers from the user. If later evidence weakens the case, revise the verdict and re-enter the product decision gate before more discovery or any implementation.

## 3. Recommend a Model Tier

After classification and a preliminary product assessment that is **proceed** or **reduce scope**, recommend a model/reasoning tier. Treat the recommendation as **advisory only** and revise it if later inspection or research changes the classification. Skip this section when the current product verdict is **decline** or **defer**, unless the user explicitly asks to continue discovery anyway.

- Recommend **low** for small, deterministic, local, easily reversible work with a narrow verification path. Suggest low or minimal reasoning when the selected model supports it.
- Recommend **medium** for bounded feature work, refactoring, or debugging that contains several decisions but has limited blast radius and straightforward rollback. Suggest medium reasoning.
- Recommend **high** for major, ambiguous, multi-step, architectural, data, API, compatibility, security, privacy, payment, concurrency, migration, or difficult-to-reverse work. Suggest high or xhigh reasoning only when the selected model supports it.

Never recommend low for a high-risk trigger merely because the code change appears short. State the recommended tier, the reason, the expected speed or token trade-off, and the condition that would justify moving up or down a tier.

Do not switch models, change reasoning settings, edit the host agent's configuration, or spawn a subagent solely to realize the recommendation. Respect the user's explicit model selection. Do not claim that a different model or tier was used unless the current environment verifies it. Keep model names out of the core workflow; when the user asks for a specific model, use current availability and official guidance rather than a hardcoded name.

If the environment cannot verify the current model or reasoning setting, say so instead of assuming it matches the recommendation. For medium or major work that passed the product decision gate, end the response at the model checkpoint and wait for confirmation before deeper discovery.

Make the checkpoint unambiguous for a non-technical user. State explicitly:

- that the model-tier recommendation is separate from the preliminary product recommendation already given;
- that GitHub research and deeper validation have not started yet;
- whether a switch is necessary when the current selection is verifiable;
- when it is not verifiable, that no switch is needed if the current selection already meets or exceeds the recommended tier;
- the exact next action: reply **continue** to keep the current selection, or switch first and then reply **continue**.

Never end with only “confirm or adjust,” and never make the user infer which recommendation they received.

Use a compact checkpoint equivalent to this, translated into the user's language:

> Recommended model tier: Medium.
>
> The product recommendation above is preliminary; GitHub research and deeper validation have not started yet.
>
> I cannot verify your current selection. If it is already medium or higher, no switch is needed.
>
> Reply **continue** to keep the current selection, or switch first and then reply **continue**.

## 4. Establish a Search Brief

Learn only enough to search safely and accurately: the problem category, target user, observable outcome, relevant platform or stack, and important constraints. If one of these would materially change the search, ask one question before researching.

Never include private code, repository names, secrets, customer data, or sensitive business details in a public query. Abstract the problem while preserving the technical constraints.

When the working request came from a **reduce scope** verdict, search for the reduced or improved direction, not the abandoned larger request.

## 5. Conduct GitHub Research

Research GitHub before comparing implementation options for every medium or major change that has cleared the product and model gates. Search relevant repositories, issues, discussions, design documents, examples, and official documentation. Prefer primary sources and open the most relevant results instead of relying on snippets.

Use a bounded research pass by default: try no more than three focused query variations and deeply review two to five of the best primary results. Stop earlier when two credible, independent sources establish the solution space and main risks, or when additional results only repeat known information. If missing project context would change the candidates, ask one material question instead of widening the search. For each useful analogue, record:

- a link and the problem it solves;
- meaningful similarities and differences;
- requirements, edge cases, or terminology it reveals;
- patterns worth adopting or avoiding;
- maintenance activity, license, compatibility, dependencies, security concerns, and adoption cost.

Do not rank by stars alone, copy code blindly, or add a dependency merely because an implementation exists. If no close analogue exists, report the queries and the absence of a useful match, then reason from first principles. If the host agent cannot access GitHub or the public web, say so and ask whether to continue without research; never imply that research occurred.

Feed research discoveries back into the interview and requirements before proposing solutions.

## 6. Interview for Material Decisions

Ask one focused question at a time. Ask only when the answer can change necessity, scope, user behavior, success criteria, design, rollout, or risk. Explain why the question matters in plain language and give a recommended default when useful.

Cover as applicable:

- target users, current pain, desired outcome, priority, and success measures;
- primary, alternate, failure, and recovery scenarios;
- included and explicitly excluded behavior;
- compatibility, accessibility, privacy, security, performance, and operational constraints;
- dependencies, rollout, monitoring, migration, and rollback expectations;
- assumptions exposed by project inspection or GitHub research.

Do not ask questions for ceremony or expect the user to resolve technical issues unaided. Translate trade-offs into user, cost, maintenance, and failure consequences. Stop when no unresolved answer would materially change the result; record reasonable defaults as explicit assumptions.

## 7. Produce a Requirements Brief

Separate **what and why** from **how**. Present a concise, technology-agnostic requirements brief containing:

- problem, evidence, target users, and desired outcome;
- user scenarios and observable behavior;
- functional and non-functional requirements;
- goals, measurable success criteria, and acceptance criteria;
- scope, non-goals, constraints, and compatibility promises;
- assumptions, dependencies, risks, recovery expectations, and open decisions;
- relevant lessons from GitHub research.

Do not hide uncertainty inside vague wording. Resolve it, state a recommended assumption, or mark it as an explicit decision still requiring the user.

## 8. Compare Solutions

Only after GitHub research and a stable requirements brief, compare two or three viable approaches, including the smallest viable option when appropriate. For each, explain user value, complexity, risk, reversibility, maintenance burden, compatibility, implementation effort, and likely token or rework cost. Recommend one option and explain why the rejected options are less suitable.

## 9. Plan and Request Approval

Present a decision-complete implementation plan scaled to risk. Include:

- selected approach, requirements, non-goals, and assumptions;
- affected components and sequence of changes;
- tests and acceptance criteria;
- migration, rollout, monitoring, and rollback where relevant;
- remaining risks or decisions.

End with an explicit approval request. Do not begin medium or major implementation until the user approves the plan or clearly authorizes proceeding with a stated assumption. If the user changes scope, repeat classification and update downstream requirements and plans.

## 10. Apply Engineering Discipline After Approval

Use relevant engineering skills available in the host agent after approval and announce each invocation. For major changes, prefer the corresponding Superpowers skills when installed; otherwise use host-native skills or apply the equivalent discipline directly:

- `using-git-worktrees` for isolated major work in a Git repository;
- `systematic-debugging` before proposing a fix for a bug or unexplained failure;
- `test-driven-development` for behavior changes and bug fixes;
- `executing-plans` for multi-step implementation with checkpoints;
- `verification-before-completion` before any completion claim;
- `requesting-code-review` for major changes or high-risk work;
- `finishing-a-development-branch` when deciding how to integrate finished branch work.

Treat these names as capability labels rather than mandatory dependencies. Do not install Superpowers or any other skill pack without permission. Invoke only skills actually available in the current session and never claim to have invoked a missing skill. If one is unavailable, disclose that fact and apply its core discipline directly when safe, or ask before installing or materially changing the workflow.

## User Overrides

Honor explicit user intent, but keep the safety boundary visible:

- If the user says to skip discovery, skip research, or “just implement,” restate the current classification and product verdict in one short paragraph, name the main risk of skipping, and ask for a clear override such as **implement anyway**. Do not silently abandon the skill.
- A medium or major override still requires an explicit implementation go-ahead before editing project files. “Continue” at the model checkpoint is not plan approval.
- If the user insists on the original larger scope after a **reduce scope** verdict, keep that insistence as an explicit assumption and raise the classification or model tier when the larger scope warrants it.
- If the user replies **continue anyway** after **decline** or **defer**, open the model checkpoint next, then proceed with discovery while keeping the negative product recommendation visible until new evidence changes it.

See [references/edge-cases.md](references/edge-cases.md) for common override and reclassification patterns.

## Token Efficiency

Reduce total tokens and rework without weakening correctness:

- bypass the full workflow for genuinely small changes;
- stop on preliminary **decline** or **defer** before research and planning;
- inspect and research only what can affect the decision;
- use at most three focused GitHub queries and deeply review no more than five results by default; stop when additional evidence has low decision value;
- ask only material questions and stop when requirements are decision-complete;
- summarize sources with links instead of pasting long code or prose;
- keep a compact record of confirmed decisions and do not repeatedly restate them;
- scale requirements and plan detail to change risk;
- implement and verify in small increments so failures are diagnosed rather than rewritten blindly;
- use a lighter path when discovery would predictably cost more than a safe direct change, and explain why.

Do not optimize for the shortest immediate response when that increases the risk of wrong code or expensive rework.

## Response Sequence

Reveal sections progressively rather than emitting empty templates:

1. **Classification** — small, medium, or major; reasons, consequences, confidence.
2. **Preliminary Product Assessment** — necessity, likely user and product value, evidence gaps, risks, smallest alternative, improvement direction, and a preliminary proceed/reduce/defer/decline verdict.
3. **Product Decision Gate** — stop on decline or defer; otherwise continue with the assessed or reduced scope.
4. **Model Tier** — advisory low, medium, or high; reason, trade-off, escalation condition. Skip after decline or defer unless the user continues anyway.
5. **Model Checkpoint** — continue without pausing for a small change; for medium or major proceed/reduce-scope work, distinguish the model advice from the preliminary product verdict, give the exact action, and stop until the user replies **continue**.
6. **Question** — the single next material unknown, if any.
7. **GitHub Findings** — evidence and lessons before solution comparison.
8. **Requirements Brief** — confirmed what and why.
9. **Options** — approaches, trade-offs, and recommendation.
10. **Plan** — decision-complete implementation sequence.
11. **Approval** — explicit permission before medium or major code changes.
