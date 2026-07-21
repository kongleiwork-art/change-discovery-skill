# Change Discovery

[中文](README.zh-CN.md) | English

An open Agent Skill that makes a coding agent question whether a requested change will actually improve the product and help its users before touching code.

Most coding agents are optimized to implement what you ask. `change-discovery` adds a product-and-engineering checkpoint first:

> Is this change necessary in this project? Which user problem does it solve? Is the expected product improvement worth the implementation and maintenance cost?

## What makes it different

Before implementation, the skill asks the agent to understand the current project and challenge the request instead of treating it as automatically valuable. It examines:

- the affected users and their real problem;
- the expected product and user-experience improvement;
- evidence that the problem exists;
- the cost of doing nothing;
- smaller, non-code, or lower-risk alternatives;
- implementation, maintenance, compatibility, and operational costs.

It then recommends **proceed**, **reduce scope**, **defer**, or **decline**.

## Workflow

1. Classify the change as small, medium, or major based on risk and blast radius—not line count alone.
2. Give a preliminary verdict on necessity and expected product value, identify missing evidence, and suggest at least one smaller or better direction.
3. Stop at a product decision gate when the verdict is **decline** or **defer**. For **proceed** or **reduce scope**, recommend an advisory low, medium, or high model/reasoning tier without switching models automatically. Small changes continue by default; medium and major proceed/reduce-scope work pauses until the user confirms the current selection or adjusts it.
4. Create a privacy-safe search brief for the working scope, including a reduced direction when that was recommended.
5. For medium and major changes that cleared the gates, research similar GitHub work before comparing solutions.
6. Ask one material question at a time so non-technical users can make informed decisions.
7. Produce a requirements brief that separates **what and why** from **how**.
8. Compare two or three viable approaches, including the smallest useful option.
9. Present an implementation plan and require explicit approval before medium or major changes.
10. After approval, use available Superpowers or equivalent engineering disciplines for implementation and verification.

## Change classification

- **Small:** local, reversible, contract-preserving work with a narrow verification path.
- **Medium:** bounded behavior change with limited dependencies and straightforward rollback.
- **Major:** changes involving data, public APIs, security, architecture, multiple systems, core user journeys, migrations, rollout, or difficult recovery.

The user does not need to decide the category. The agent must classify it and explain the practical consequences.

## Supported agents

The core `SKILL.md` follows the portable Agent Skills format and can be used by compatible hosts such as:

- OpenAI Codex
- Claude Code
- Cursor
- GitHub Copilot
- Gemini CLI

`agents/openai.yaml` provides optional OpenAI UI metadata. Other hosts can ignore it.

## Installation

Clone the repository into the personal or project skill directory used by your agent. Common personal locations include:

| Agent | Example personal location |
| --- | --- |
| Codex | `~/.codex/skills/change-discovery` |
| Claude Code | `~/.claude/skills/change-discovery` |
| Cursor | `~/.cursor/skills/change-discovery` |
| GitHub Copilot | `~/.copilot/skills/change-discovery` or `~/.agents/skills/change-discovery` |
| Gemini CLI | `~/.gemini/skills/change-discovery` or `~/.agents/skills/change-discovery` |

Example for Codex:

```bash
git clone https://github.com/kongleiwork-art/change-discovery-skill.git \
  ~/.codex/skills/change-discovery
```

Use the equivalent path for another host. Restart or reload the agent if it does not discover the skill immediately.

## Usage

The agent may invoke the skill automatically when a proposed change needs discovery. You can also request it explicitly:

```text
Use change-discovery to evaluate this request before changing code:
Add a community feature to my application.
```

Expected behavior begins with classification, a preliminary product-value verdict, and an improvement direction—not implementation. If the verdict is decline or defer, the agent stops before model selection and research. If the verdict is proceed or reduce scope, it gives model-tier advice next. Small changes state that they will continue with the current selection. Medium and major proceed/reduce-scope work stops at a model checkpoint before GitHub research and deeper discovery continue.

At that checkpoint, the agent must distinguish the preliminary product verdict from the model advice, say that deeper validation has not started, and give one clear action: reply `continue` to keep the current model, or switch first and then reply `continue`. After decline or defer, the clear action is `continue anyway` only if the user still wants discovery.

## Safety and limitations

- Model-tier recommendations are advisory; the skill never switches models or reasoning settings by itself.
- The model checkpoint adds no extra confirmation round for small changes unless the user requests one.
- Decline and defer stop before model selection and GitHub research unless the user explicitly continues anyway.
- A model-tier recommendation must be clearly separated from the preliminary recommendation to proceed, reduce scope, defer, or decline.
- “Continue” at the model checkpoint is not approval to edit project files.
- GitHub research requires the host to have public web or GitHub access.
- The approval gate is an instruction-level guardrail, not an operating-system permission boundary.
- Superpowers is optional. The agent must use equivalent engineering discipline when those skills are unavailable.
- Public research must not expose private code, repository names, secrets, customer data, or sensitive business information.

## License

[MIT](LICENSE)
