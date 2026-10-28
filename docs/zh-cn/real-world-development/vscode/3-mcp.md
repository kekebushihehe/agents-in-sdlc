---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚才使用智能体模式构建了筛选功能。创建拉取请求之前，应确认功能在浏览器中正常工作。接下来连接 **Playwright 模型上下文协议（MCP）服务器**，让 Copilot 操作真实浏览器测试功能，而不必手动逐项操作，然后发布分支并创建拉取请求（PR）。

本练习将完成以下任务：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

当智能体模式能够访问编辑器之外的内容时，其功能会更强大。Copilot 通过模型上下文协议（MCP）实现这一点：这是一种让智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作原理示意图，展示它如何与上下文、大语言模型（LLM）和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)让人工智能（AI）智能体能够与外部工具和服务通信。使用 MCP 后，AI 智能体可以实时与外部工具和服务通信，从而获取最新信息（使用资源）并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理双方之间的通信（例如通过现有应用程序编程接口（API）或 NPM 包等本地工具）。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体可以创建新存储库、更新现有存储库以及管理 issue 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 智能体可以打开网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可用于访问不同的工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，方便大家查找并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用 MCP 服务器之前，请仔细审查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应会看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含 `github` 条目，因此 Copilot 能在上一练习中读取 issue 待办事项。此条目使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容；VS Code 会通过 HTTP 连接服务器，Copilot 首次使用服务器工具时需要使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]让 Copilot 能够操控浏览器，正好可用于测试刚实现的功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器；codespace 中没有桌面环境，因此必须使用此设置。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要服务器工具时启动服务器，并询问是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操控的真实浏览器。无需亲自操作应用来检查工作，智能体可以打开网页、导航、应用筛选条件、读取结果并总结观察到的内容。无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而非屏幕截图。这样智能体就可以像辅助技术一样，根据结构化且带标签的元素（按钮、链接、列表项）进行推理，因此快速功能检查也可以顺便验证基本无障碍情况。

服务器已连接且应用正在运行时，让 Copilot 测试刚完成的筛选功能：

```text
使用 Playwright MCP 服务器，在运行的应用 http://localhost:4321 中打开浏览器并验证新游戏筛选功能：

1. 转到游戏页面，记下列出的游戏数量。
2. 应用类别筛选，并确认列表只显示该类别的游戏。
3. 清除筛选，然后应用出版社筛选，并确认列表只显示该出版社的游戏。
4. 同时使用类别和出版社筛选，并确认结果同时符合两个条件。

报告每个步骤中的观察结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，执行每个步骤并报告结果。根据 issue 中的验收标准审查其总结；如果发现异常，可以提出后续问题，或在创建拉取请求前让它修复代码。

> [!NOTE]
> 测试时，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，包括使用 github.com 和 GitHub 命令行界面（CLI）。既然正在使用 GitHub Copilot，不妨让它代为创建 PR。它可以找到相关 issue 并创建与 issue 关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉列表中选择了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   在存储库中查找与按类别和出版社筛选相关的 issue。为当前分支创建新的拉取请求，并将其关联到正确的 issue。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你进行 GitHub 身份验证；按照提示完成操作。
5. 留意 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜你！本练习完成了以下任务：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

你使用 MCP 服务器**测试**了一项功能，不过 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
