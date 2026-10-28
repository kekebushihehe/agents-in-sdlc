---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前面的几次练习中，你让 Copilot 云智能体处理了三项任务，旨在改进用户体验并添加功能。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有包含未关闭拉取请求（PR）的智能体任务。你可以查看智能体执行的操作，甚至引导正在进行的会话。

本课将完成以下任务：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，加入这项功能就很合理。现在要引导智能体，让它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 转到 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 为修改游戏生成应用程序编程接口（API）。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的任务，例如：
    - 从存储库检出代码。
    - 创建 Copilot 工作环境。
    - 设置模型上下文协议（MCP）服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和采用的方法而变化。

4. 另请留意右侧出现的拉取请求（PR）窗格。可以在其中查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

了解当前活动任务后，接下来在智能体处理高对比度模式时请求它也添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 采用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。出现类似下方的消息时，就说明已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    添加高对比度模式的同时，也添加浅色模式。还应提供一个浅色模式开关，让用户选择所需的显示模式。
    ```

    ![智能体页面中云智能体任务的截图，其中高亮显示了 steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！完成收到消息时正在执行的具体步骤后，它会将新请求纳入工作流程。

与之前一样，这需要几分钟，可以借此机会回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中央位置。借助此任务控制中心，可以跟踪所有活动和已完成的任务、审查正在进行的工作，还可以调整正在进行任务的范围或提供额外指导。

你了解了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 引导正在进行的会话添加功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进它创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
