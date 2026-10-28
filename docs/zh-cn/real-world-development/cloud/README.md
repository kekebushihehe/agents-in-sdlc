---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)可在云端异步处理任务。你可以在 GitHub 上分配工作，云智能体便会在后台接手任务、探索存储库、进行更改并创建拉取请求，让你可以继续处理其他工作。

在这一系列练习中，你将添加云智能体会遵循的自定义指令，然后分配 GitHub issue 让它实现功能。你将审查并使用自定义智能体来指导它的工作方式，在智能体仪表板中监控并引导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云智能体将遵循的自定义指令 |
| [2. 云智能体][ex2] | 异步智能体 | 将 issue 分配给 Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查拉取请求（PR）、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education]，经验证的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的智能体、模型上下文协议（MCP）、代码审查和 Copilot CLI 功能，因此可用它完成所有环境中的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，所有计划（Copilot Student、Pro、Pro+、Business 和 Enterprise）都支持云智能体。对于 Copilot Business 或 Enterprise，管理员必须为组织启用此功能。

> [!NOTE]
> 本环境不介绍模型上下文协议（MCP）。若要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 环境](../cli/)或 [VS Code 环境](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
