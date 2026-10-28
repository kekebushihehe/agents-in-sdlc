---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合 issue 要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright 的模型上下文协议（MCP）服务器在浏览器中查看筛选体验。现在将这些能力整合到一个具有明确定义质量保证（QA）流程的自定义智能体中。

本课将完成以下任务：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查可重复使用的质量保证配置文件。
- 选择 QA 智能体，根据筛选 issue 审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调这一 QA 流程，并生成可重复使用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

使用过的自定义项各有不同用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何使用这些能力评估需求并报告发现。它不会取代其他自定义项，也不需要启动另一个对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建可重复使用的 QA 配置文件。配置文件将定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义智能体。它应根据 issue 和已达成一致的要求检查功能、遵循存储库指令、运行 quality-checks 技能、使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   它应将每项要求标记为通过、失败或受阻，并提供支持证据。修改实现代码前必须先询问我，且不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新智能体前，先检查其配置文件，确认 Copilot 捕获了预期的 QA 流程和边界。这样可避免在只需要验证功能时，让不完整或范围过宽的智能体修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需字段；`name` 是可选字段，但添加后可以为智能体提供清晰的显示名称。
3. 阅读配置文件指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少上述职责或边界，请先让默认智能体修改，然后再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一个筛选对话，再选择 QA，这样它就能使用已有的 issue 和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中。它仍然可用，上面的命令会启用它。

2. 使用以下提示，让 QA 智能体审查功能：

   ```plaintext
   根据 issue 和计划中商定的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. QA 智能体会开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已在工作流中添加可重复使用的专用角色，并审查了它的工作。本课中，你：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查可重复使用的质量保证配置文件。
- 选择 QA 智能体，根据筛选 issue 审查其发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好进行审查。接下来，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
