---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "浏览现有的 quality-checks 技能，自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证代码是否正常工作，并使用指令文件确保代码符合标准。那么测试和 lint 呢？持续集成（CI）的其他部分又该如何处理？

**智能体技能**非常适合这类任务！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将完成以下任务：

- 浏览现有的 `quality-checks` 技能。
- 自定义技能结果的格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，使结果更易于阅读。

## 指令、脚本和资源

智能体技能将可重复使用的任务指令、可执行脚本和支持资源打包在一起，由智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。Markdown 文件包括用于定义技能名称和说明的 frontmatter、技能功能概述，以及调用技能的指导。文件夹中还可以包含供技能使用的脚本和其他资源子文件夹。

> [!NOTE]
> 技能不一定需要额外的文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和重复使用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 浏览技能

接下来查看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的技能 `quality-checks`。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明有助于 Copilot 判断何时调用技能。
3. 阅读指令，留意它们如何引导 Copilot 执行测试和 lint 流程。
4. 留意技能中尚未包含**结果输出格式**部分。

## 更改前先运行技能

技能可以通过 Copilot CLI 直接调用，也可以使用自然语言调用。现在让 Copilot 运行三项检查。

1. 返回处于 Interactive 模式的筛选对话。
2. 使用以下提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告。

## 自定义报告

我们希望报告能清楚说明运行了什么、是否成功，以及工具实际报告的内容。现在更新技能，生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## 结果输出格式

   完成后，报告运行的每条命令，并标明其通过、失败或受阻。仅在工具提供了相关信息时，才包括测试数量、持续时间、错误、警告和其他指标。对任何失败或阻塞项指出后续操作，绝不要将跳过或未完成的检查描述为通过。
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改已完成，现在查看效果。Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告，并与第一份报告比较。

## 总结和后续步骤

你已自定义并使用现有智能体技能。本课中，你：

- 浏览了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的格式。
- 重新加载并运行了技能。

此更改会与筛选功能一起包含在功能 PR 中。接下来，[使用 Playwright 模型上下文协议（MCP）服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是需要完成的任务：

- [智能体技能规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
