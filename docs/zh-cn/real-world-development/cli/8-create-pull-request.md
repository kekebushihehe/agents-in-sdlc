---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都保存在同一分支上。现在需要一起审查并创建拉取请求。你之前手动合并了星级评分拉取请求（PR）；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本研讨会，你在同一个对话和分支中完成了筛选和质量工作，并将所有更改放在此 PR 中。

本课将完成以下任务：

- 了解 Agent Merge 及其如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流程中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动化剩余的 PR 工作，同时让开发者控制合并授权。

## 介绍 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻塞 PR 的问题，包括修复失败的持续集成（CI）检查、回应审查意见，以及必要时进行变基；之后，如果存储库支持，就会启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代为处理这项工作。启用 Agent Merge 前，先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码已全部完成，现在一起审查代码、创建 PR，并让 Agent Merge 管理其余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认看到 Copilot 显示类似 `PR #14 was squash-merged successfully.` 的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限不足等限制。如果任务受阻，请阅读报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动化开发流程的多个部分，包括代码生成、测试和验证，以及拉取请求流程。本课中，你：

- 了解了 Agent Merge 及其如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
