---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并使用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还需要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。模型上下文协议（MCP）服务器可以让 Copilot 超越 CLI 内置功能，访问更多工具和服务。

本课将完成以下任务：

- 了解模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加它。
- 让智能体操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试都很重要，但验证用户界面（UI）更新还需要实际与 UI 交互。希望让 Copilot 像用户一样操作正在开发的网站，进一步自动化更改流程，并增强对功能符合预期的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]让人工智能（AI）智能体能够与外部工具和服务通信。使用 MCP 后，AI 智能体可以实时获取最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、issue 和拉取请求的应用程序编程接口（API）。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 托管了 [MCP 注册表][mcp-registry]，方便大家查找并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能正常。现在使用 Copilot 中的 Playwright MCP 服务器进行同样的测试。

1. 使用以下提示，让 Copilot 调用 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用并使用 Playwright MCP，根据 issue 和我们的计划检查筛选功能。告诉我哪些功能正常、哪些不正常，但不要进行任何更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 实际上无需明确告诉 Copilot 使用 MCP 服务器，因为它通常会自行判断。但既然知道该用什么工具，明确指出有助于获得更一致的结果，也能节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的各个步骤，以确认功能正常。
3. 阅读报告并确认所有行为符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器并提供报告。

## 总结和后续步骤

恭喜你！你已从 Copilot CLI 使用 Playwright MCP 服务器在真实浏览器中探索功能。回顾一下，你：

- 了解了模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加了它。
- 让智能体操作浏览器并探索筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
