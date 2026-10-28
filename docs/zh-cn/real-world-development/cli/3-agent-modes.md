---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方案，使用 Autopilot 根据 issue 构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚才为项目添加了一个小功能。不过，更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流程，帮助团队以正确的方式构建正确的功能。本课将开始一系列智能体驱动开发流程练习：从 issue 入手生成新功能，确保代码有效、功能符合预期，并最终成功合并到项目中。

> [!NOTE]
> 后续功能工作将使用相同的对话和分支。通常不同类型的文件会使用不同分支或拉取请求（PR），但本研讨会会略过这些步骤，以便聚焦核心概念。

本课将完成以下任务：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式下定义要求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加模型上下文协议（MCP）验证、创建质量保证（QA）智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断增长，访问者需要按类别和出版社缩小游戏范围。待办事项中的 issue 描述了该功能，但组合筛选等细节需要在编码前确认。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

将人工智能（AI）编码智能体引入开发流程并不会改变基本原则。事实上，这些原则变得更加重要。多数开发者的工作流程大致如下：

1. 从描述需求的 issue 开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

坚持采用标准流程，可以确保 AI 生成的代码满足既定要求，并像手动编写的代码一样经过审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先创建计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并不断迭代，直到任务完成。

先使用 Plan 模式，再审查计划，然后使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能工作前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动后再继续。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main` 并为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中并返回 **Session** 选项卡。

留意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub issue 或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划可以先确定功能实现方式，再交给 Copilot 执行。对于复杂任务，花些时间规划通常很有帮助。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   制定实现此功能的计划。
   ```

Copilot 会先探索项目，再确定最佳实现方案并生成计划。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已编写并审查完毕，现在开始实现。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代问题，直到它认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所安装版本中类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，不要将检查视为已完成。

## 审查并验证实现

代码生成完成后，应像其他代码一样在合并前进行审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与 issue 以及规划阶段做出的决定进行比较。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决任何失败项。

## 探索新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告诉我 URL 并让服务器保持运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按出版社筛选，以及计划中确定的组合方式。
4. 确认重置筛选和无结果时的行为符合 issue 和规划阶段的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中，你：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式下定义要求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证功能。

接下来，[使用自定义指令][next-lesson]深入了解代码生成，确保代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
