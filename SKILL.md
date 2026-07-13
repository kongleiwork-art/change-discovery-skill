---
name: change-discovery
description: Classify and clarify proposed code changes before implementation, especially when the user cannot judge scope or a request may affect architecture, user flows, data, APIs, dependencies, security, performance, deployment, or multiple components. Determine whether the change is small, medium, or major; recommend an advisory low, medium, or high model/reasoning tier without switching models; challenge necessity; research similar GitHub work before comparing solutions; produce a requirements brief and approval-gated plan; then apply relevant engineering disciplines using capabilities available in the host agent.
---

# Change Discovery

## Purpose

Help a non-technical user decide what is worth building and turn an uncertain request into clear requirements and a safe engineering plan. Optimize for correct total effort and low rework, not for the fewest discovery tokens.

## Host Compatibility

Treat this workflow as host-independent. Use the current agent's native tools for project inspection, public research, questions, planning, and approval. Adapt tool and skill names instead of assuming a particular vendor, model family, filesystem layout, or interaction UI.

Preserve the safety boundaries even when the host lacks a dedicated read-only, planning, or approval mode. State capability limitations instead of pretending that a tool, skill, model tier, or research action is available. Ignore host-specific metadata files that the current agent does not recognize.

## Hard Gate

Decide the change size yourself; never ask the user to classify it. Before the user explicitly approves a medium or major change, restrict work to read-only project inspection, public research, questions, requirements, options, and an in-chat plan. Do not edit project files, write code, create implementation artifacts, install dependencies, commit, push, or make external write actions.

For a small change, state the classification and reason, then follow the normal implementation workflow. If scope is uncertain, use the higher provisional class until one material answer resolves it.

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

## Recommend a Model Tier

Immediately after classification, recommend a model/reasoning tier. Treat the recommendation as **advisory only** and revise it if later inspection or research changes the classification.

- Recommend **low** for small, deterministic, local, easily reversible work with a narrow verification path. Suggest low or minimal reasoning when the selected model supports it.
- Recommend **medium** for bounded feature work, refactoring, or debugging that contains several decisions but has limited blast radius and straightforward rollback. Suggest medium reasoning.
- Recommend **high** for major, ambiguous, multi-step, architectural, data, API, compatibility, security, privacy, payment, concurrency, migration, or difficult-to-reverse work. Suggest high or xhigh reasoning only when the selected model supports it.

Never recommend low for a high-risk trigger merely because the code change appears short. State the recommended tier, the reason, the expected speed or token trade-off, and the condition that would justify moving up or down a tier.

Do not switch models, change reasoning settings, edit the host agent's configuration, or spawn a subagent solely to realize the recommendation. Respect the user's explicit model selection. Do not claim that a different model or tier was used unless the current environment verifies it. Keep model names out of the core workflow; when the user asks for a specific model, use current availability and official guidance rather than a hardcoded name.

## 2. Challenge the Request

Before designing a solution, state:

- the problem, affected user, and intended outcome as understood;
- evidence that the problem exists, or the assumption still needing validation;
- the cost of doing nothing;
- the smallest viable option, including non-code or lower-risk alternatives;
- expected value, risk, reversibility, and likely scope.

Recommend **proceed**, **reduce scope**, **defer**, or **decline**. Do not treat the original request as automatically necessary.

## 3. Establish a Search Brief

Learn only enough to search safely and accurately: the problem category, target user, observable outcome, relevant platform or stack, and important constraints. If one of these would materially change the search, ask one question before researching.

Never include private code, repository names, secrets, customer data, or sensitive business details in a public query. Abstract the problem while preserving the technical constraints.

## 4. Conduct GitHub Research

Research GitHub before comparing implementation options for every medium or major change. Search relevant repositories, issues, discussions, design documents, examples, and official documentation. Prefer primary sources and open the most relevant results instead of relying on snippets.

Use a bounded research pass by default: try no more than three focused query variations and deeply review two to five of the best primary results. Stop earlier when two credible, independent sources establish the solution space and main risks, or when additional results only repeat known information. If missing project context would change the candidates, ask one material question instead of widening the search. For each useful analogue, record:

- a link and the problem it solves;
- meaningful similarities and differences;
- requirements, edge cases, or terminology it reveals;
- patterns worth adopting or avoiding;
- maintenance activity, license, compatibility, dependencies, security concerns, and adoption cost.

Do not rank by stars alone, copy code blindly, or add a dependency merely because an implementation exists. If no close analogue exists, report the queries and the absence of a useful match, then reason from first principles. If the host agent cannot access GitHub or the public web, say so and ask whether to continue without research; never imply that research occurred.

Feed research discoveries back into the interview and requirements before proposing solutions.

## 5. Interview for Material Decisions

Ask one focused question at a time. Ask only when the answer can change necessity, scope, user behavior, success criteria, design, rollout, or risk. Explain why the question matters in plain language and give a recommended default when useful.

Cover as applicable:

- target users, current pain, desired outcome, priority, and success measures;
- primary, alternate, failure, and recovery scenarios;
- included and explicitly excluded behavior;
- compatibility, accessibility, privacy, security, performance, and operational constraints;
- dependencies, rollout, monitoring, migration, and rollback expectations;
- assumptions exposed by project inspection or GitHub research.

Do not ask questions for ceremony or expect the user to resolve technical issues unaided. Translate trade-offs into user, cost, maintenance, and failure consequences. Stop when no unresolved answer would materially change the result; record reasonable defaults as explicit assumptions.

## 6. Produce a Requirements Brief

Separate **what and why** from **how**. Present a concise, technology-agnostic requirements brief containing:

- problem, evidence, target users, and desired outcome;
- user scenarios and observable behavior;
- functional and non-functional requirements;
- goals, measurable success criteria, and acceptance criteria;
- scope, non-goals, constraints, and compatibility promises;
- assumptions, dependencies, risks, recovery expectations, and open decisions;
- relevant lessons from GitHub research.

Do not hide uncertainty inside vague wording. Resolve it, state a recommended assumption, or mark it as an explicit decision still requiring the user.

## 7. Compare Solutions

Only after GitHub research and a stable requirements brief, compare two or three viable approaches, including the smallest viable option when appropriate. For each, explain user value, complexity, risk, reversibility, maintenance burden, compatibility, implementation effort, and likely token or rework cost. Recommend one option and explain why the rejected options are less suitable.

## 8. Plan and Request Approval

Present a decision-complete implementation plan scaled to risk. Include:

- selected approach, requirements, non-goals, and assumptions;
- affected components and sequence of changes;
- tests and acceptance criteria;
- migration, rollout, monitoring, and rollback where relevant;
- remaining risks or decisions.

End with an explicit approval request. Do not begin medium or major implementation until the user approves the plan or clearly authorizes proceeding with a stated assumption. If the user changes scope, repeat classification and update downstream requirements and plans.

## 9. Apply Engineering Discipline After Approval

Use relevant engineering skills available in the host agent after approval and announce each invocation. For major changes, prefer the corresponding Superpowers skills when installed; otherwise use host-native skills or apply the equivalent discipline directly:

- `using-git-worktrees` for isolated major work in a Git repository;
- `systematic-debugging` before proposing a fix for a bug or unexplained failure;
- `test-driven-development` for behavior changes and bug fixes;
- `executing-plans` for multi-step implementation with checkpoints;
- `verification-before-completion` before any completion claim;
- `requesting-code-review` for major changes or high-risk work;
- `finishing-a-development-branch` when deciding how to integrate finished branch work.

Treat these names as capability labels rather than mandatory dependencies. Do not install Superpowers or any other skill pack without permission. Invoke only skills actually available in the current session and never claim to have invoked a missing skill. If one is unavailable, disclose that fact and apply its core discipline directly when safe, or ask before installing or materially changing the workflow.

## Token Efficiency

Reduce total tokens and rework without weakening correctness:

- bypass the full workflow for genuinely small changes;
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
2. **Model Tier** — advisory low, medium, or high; reason, trade-off, escalation condition.
3. **Assessment** — proceed, reduce scope, defer, or decline.
4. **Question** — the single next material unknown, if any.
5. **GitHub Findings** — evidence and lessons before solution comparison.
6. **Requirements Brief** — confirmed what and why.
7. **Options** — approaches, trade-offs, and recommendation.
8. **Plan** — decision-complete implementation sequence.
9. **Approval** — explicit permission before medium or major code changes.
