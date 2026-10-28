---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你使用 GitHub Copilot 完成了多项旨在改进用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式开关在内的无障碍功能。现在查看代码更改；如有需要，可以向 Copilot 提供反馈，进一步改进成果。

### 场景

如前所述，加入生成式人工智能（AI）并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循常规 DevOps 流程。基于这一原则，在为团队启用审查之前，先查看 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## 安全性与 GitHub Copilot 云智能体

Copilot 云智能体会异步且无人监督地执行任务，因此需要遵守一些安全限制：

- Copilot 只能读取存储库，并且只能写入它将用于编写代码的分支。
- 云智能体在 GitHub Actions 中运行，并在其中创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- [默认情况下，对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议（MCP）服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），其中为代码添加了文档。通过 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。**这是预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。你可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息片刻或回顾目前学到的内容。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新添加的 TypeScript 文档注释（TSDoc）和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查部分排队。顺利的话，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像提及团队成员一样在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新的会话来处理请求。由于结果具有非确定性，无法提供唯一固定的提示文本。可以要求 Copilot 更新以下内容：

- 为每个代码文件开头添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论请求 Copilot 更改生成的文档，并像提及任何用户一样提及 **@copilot**。可以使用上面的建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 执行工作。留意它如何启动新会话来进行更新。
3. 选择 **Back to pull request** 可返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。之后，将 PR 从草稿转换为可审查状态，并合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为解决“在游戏详情页显示相关游戏”问题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入拉取请求的检查部分排队。顺利的话，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：** 也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，可以迭代改进云智能体的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加评论提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，查看使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意以下方面：
   - 用于切换模式的用户界面（UI）组件。
   - 使用本地存储持久化用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（无障碍富互联网应用（ARIA）标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查部分排队。顺利的话，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式开关，确认功能正常，且页面重新加载后设置仍然保留。

留意无障碍自定义智能体如何帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像处理前面 PR 时一样在评论中提及 **@copilot**，请求更新。

## 可选练习 — 继续委派

云智能体适合处理真实的待办事项，让你可以专注于其他工作。可以在自己的存储库中创建一些 issue 并分配给 Copilot，养成这种工作习惯。以下是一些建议：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体环境中的练习。在这些课程中，你：

- **检查了此存储库随附的自定义指令文件**，以便之后查看它们对云智能体输出的影响。
- **将 issue 分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**处理无障碍问题，添加高对比度和浅色模式开关。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导正在进行的无障碍会话。
- **审查并迭代改进云智能体的拉取请求**，提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体环境的练习。如果想继续探索，其他环境中的练习可以补充本次实践：

- 🖥️ **[VS Code 环境](../../vscode/)** — 直接在集成开发环境（IDE）中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义质量保证（QA）智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
