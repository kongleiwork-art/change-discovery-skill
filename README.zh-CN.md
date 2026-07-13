# Change Discovery

中文 | [English](README.md)

一个开源的 Agent Skill：让编程 Agent 在碰代码之前，先反问这个需求放在当前项目里是否真的能提升产品、帮助用户。

多数编程 Agent 擅长执行你提出的需求。`change-discovery` 会先增加一道产品与工程决策关口：

> 这个改动在当前项目里真的有必要吗？它解决了哪个用户问题？预期的产品提升，值得开发和长期维护成本吗？

## 核心特点

开始实施前，它会先了解当前项目并质疑需求，而不是默认每个需求都有价值。它会分析：

- 受影响的用户和他们真正遇到的问题；
- 对产品和用户体验的预期提升；
- 是否有证据证明问题存在；
- 什么都不做会造成什么影响；
- 是否存在更小、非代码或风险更低的替代方案；
- 开发、维护、兼容和运营成本。

分析后，它会明确建议：**继续、缩小范围、暂缓或放弃**。

## 工作流程

1. 根据风险和影响范围自动判断小改、中改或大改，而不是只看代码行数。
2. 推荐低、中、高模型或推理档位，但不会自动切换模型。小改默认继续；中改和大改必须暂停，等待用户确认继续使用当前选择或完成调整。
3. 质疑需求的必要性和预期产品价值。
4. 生成经过脱敏的检索摘要。
5. 中改和大改在比较方案前，先去 GitHub 调研类似实现。
6. 每次只问一个会影响决策的关键问题，帮助不懂技术的用户逐步梳理需求。
7. 输出需求简报，把“做什么、为什么做”和“怎么实现”分开。
8. 比较两到三个可行方案，包括最小可用方案。
9. 给出实施计划；中改和大改必须得到明确批准后才能碰代码。
10. 批准后调用可用的 Superpowers 技能，或执行等价的工程纪律和验证流程。

## 如何判断改动规模

- **小改：** 局部、可逆、不改变外部约定，验证路径简单。
- **中改：** 在有限组件内改变行为，依赖较少，容易回滚。
- **大改：** 涉及数据、公共 API、安全、架构、多个系统、核心用户流程、迁移、发布或困难恢复。

用户不需要自己判断属于哪一类。Agent 必须自行分类，并用容易理解的方式说明实际影响。

## 支持的 Agent

核心 `SKILL.md` 采用可移植的 Agent Skills 格式，可以用于支持该格式的宿主，例如：

- OpenAI Codex
- Claude Code
- Cursor
- GitHub Copilot
- Gemini CLI

`agents/openai.yaml` 只提供可选的 OpenAI 界面元数据，其他 Agent 可以忽略。

## 安装

将仓库克隆到对应 Agent 的个人或项目 Skill 目录。常见个人目录如下：

| Agent | 个人目录示例 |
| --- | --- |
| Codex | `~/.codex/skills/change-discovery` |
| Claude Code | `~/.claude/skills/change-discovery` |
| Cursor | `~/.cursor/skills/change-discovery` |
| GitHub Copilot | `~/.copilot/skills/change-discovery` 或 `~/.agents/skills/change-discovery` |
| Gemini CLI | `~/.gemini/skills/change-discovery` 或 `~/.agents/skills/change-discovery` |

以 Codex 为例：

```bash
git clone https://github.com/kongleiwork-art/change-discovery-skill.git \
  ~/.codex/skills/change-discovery
```

其他 Agent 替换成对应目录即可。如果没有立即发现 Skill，请重启或重新加载 Agent。

## 使用方式

当需求需要前置分析时，Agent 可以自动调用它，也可以手动指定：

```text
使用 change-discovery，在修改代码之前评估这个需求：
给我的应用增加一个社区功能。
```

正确的开始方式应该是先判断改动规模并推荐模型档位，而不是立即写代码。小改会说明继续使用当前选择；中改和大改会停在模型确认门，确认后才继续产品需求分析。

在这个确认门中，Agent 必须明确说明它**还没有判断这个需求是否应该做**，并给出一个清楚的动作：保留当前模型就回复“继续”，需要切换则切换后再回复“继续”。

## 安全边界与限制

- 模型档位只是建议；Skill 不会自行切换模型或推理设置。
- 小改默认不增加模型确认轮次，除非用户明确要求所有改动都暂停。
- 模型档位建议不得被表达成“应该做或不应该做这个需求”的产品建议。
- GitHub 调研需要宿主 Agent 具备公共网络或 GitHub 访问能力。
- 批准门属于指令层约束，不等同于操作系统权限隔离。
- Superpowers 不是必需依赖；不可用时，Agent 必须执行等价的工程纪律。
- 公开检索不得泄露私有代码、仓库名称、密钥、客户数据或敏感业务信息。

## 许可证

[MIT](LICENSE)
