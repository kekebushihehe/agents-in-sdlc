---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]指导代码生成。

本练习将完成以下任务：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定的指令文件传递给 Copilot。
- 在当前指令下生成筛选功能的第一部分数据层代码（出版社辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能代码提交到 `main`，让 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设定的某些标准。Copilot 的行为具有非确定性。本练习的重点是观察更新指令前后行为的总体变化趋势，而不是让输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用暗色模式并具有现代感。
- 应使用 TypeScript 文档注释（TSDoc）为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好设置，帮助它了解你的编码风格和要求。借助此功能，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建指令，以提供任务级上下文。

指令文件有两种类型：

- `.github/copilot-instructions.md` 是一个指令文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，即与发送给 Copilot 的大多数请求相关的上下文。
- `.github/instructions/*.instructions.md` 文件可针对特定语言、文件类型或任务提供指南。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

本研讨会不详细介绍如何创建指令文件，但示例项目展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 中的指令集中在项目级指导上，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指导。
- 使用自然语言，保持说明清楚，并提供代码应当如何编写以及不应如何编写的示例。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过实践，找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览此项目中的文件时，你可能会发现有些文件针对多种任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；本研讨会会在相关课程中介绍对应方法。
>
> 想查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库随附的指令文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目的简要说明以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子节。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。留意针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置一个相对于存储库根目录的 glob，用于确定指令适用的文件。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 留意此项目中有关创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小且可重复使用的文件，并为 Copilot 提供可遵循的示例。（这些路径相对于指令文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求在代码中添加文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习所用的模式和模型可能不同；每个步骤会在相关处说明。

先在当前指令文件下发送提示，观察自定义指令的影响。稍后更新指令后，再发送相同提示并比较结果。

1. 关闭前面练习留下的编辑器选项卡，确保 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，这样 Copilot 才能应用文件更改。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有出版社的名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目，找到相关文件并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的出版社数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有非确定性，因此即使没有相关指令，它也可能添加文档注释。这种情况也没问题；本练习重点是观察更新指令后行为的一致性是否提高。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来添加项目编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应在第 27 行附近）。留意它如何记录项目编码标准，但尚未包含代码文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应包含 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，为文件添加注释块，说明文件用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指令，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚添加的文档注释规则。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话缓冲区。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有出版社的名称和 ID。应用文件更改。
   ```

4. 留意建议文件现在以类似下面的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的出版社数据访问辅助函数。
    * 提供从数据库检索出版社信息的函数。
    */
   ```

5. 留意建议函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有出版社及其 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析结果为出版社对象数组。
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习将在此代码基础上继续构建。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，也需要使用源代码管理。接下来提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新以及 Copilot 为辅助函数基础所做的所有文件更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也将这些文件添加到暂存区。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求（PR）并关联待办事项中的文档标准 issue。可以手动操作，不过也可以让 Copilot 使用已连接到项目的 GitHub 工具查找 issue、创建 PR 并关闭相应 issue。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已经将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成操作。稍后的练习将介绍此连接的工作方式，即模型上下文协议（MCP）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选择了 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的 issue，并从当前分支创建 PR，说明指令更新和新的出版社辅助函数基础：

   ```
   查找与更新指令文件相关的 issue。从当前分支创建 PR，说明该 PR 会关闭此 issue，并提及我们也为后续筛选工作添加了出版社辅助函数基础。
   ```

4. Copilot 会查找 issue 并创建 PR。
5. 如果系统提示 **Allow** Copilot 代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. 创建 PR 后，让 Copilot 合并 PR 并将本地分支切回 `main`：

   ```
   将 PR 合并到 main。然后在本地切回 main 并拉取最新代码，确保代码已同步。
   ```

7. 如果系统提示 **Allow** Copilot 代表你执行 GitHub 操作和 shell 命令，请审查命令并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何读取指令文件中的上下文，并在 VS Code 中使用 Copilot Chat 完成了以下任务：

- 发送代码生成提示，观察 Copilot 在**现有**指令下生成的内容。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新运行相同提示，观察建议的代码如何采用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，使用[智能体模式][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
