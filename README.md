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
2. Recommend an advisory low, medium, or high model/reasoning tier without switching models automatically.
3. Challenge the necessity and expected product value of the request.
4. Create a privacy-safe search brief.
5. For medium and major changes, research similar GitHub work before comparing solutions.
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

Expected behavior begins with classification, model-tier advice, product-value assessment, and the next material question—not implementation.

## Safety and limitations

- Model-tier recommendations are advisory; the skill never switches models or reasoning settings by itself.
- GitHub research requires the host to have public web or GitHub access.
- The approval gate is an instruction-level guardrail, not an operating-system permission boundary.
- Superpowers is optional. The agent must use equivalent engineering discipline when those skills are unavailable.
- Public research must not expose private code, repository names, secrets, customer data, or sensitive business information.

## License

[MIT](LICENSE)
