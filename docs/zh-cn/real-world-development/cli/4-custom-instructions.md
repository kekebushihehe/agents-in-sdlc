---
title: "第 4 课 - 使用自定义指令指导 Copilot"
description: "浏览存储库指令，添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要按照特定方式完成，就应确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅说明你想要什么代码，也说明代码应如何组织。现在已经构建了筛选功能，接下来将查看 Copilot 使用的指令、添加文档标准并将其应用到代码中。

本课将完成以下任务：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TypeScript 文档注释（TSDoc）/JavaScript 文档注释（JSDoc）说明用途、参数和返回值（包括存在时可注入的 `db` 参数）。
- 可重复使用的 Astro 组件应记录其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

通过指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好设置，帮助它了解你的编码风格和要求。借助此功能，可以指定偏好的编码约定、库和注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种类型：

- `.github/copilot-instructions.md` 是一个指令文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 文件可针对特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

入门项目已包含一组指令文件。先浏览现有文件，再进行更改，以便观察更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目的简要说明和编码指导。这些指令适用于本存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。留意针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置一个 glob，用于确定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成较小且可重复使用的文件，并为 Copilot 提供可遵循的示例。

## 更新指令文件以符合团队指导

现有文件是不错的起点，但仍缺少一项内容。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（应在第 35 行附近）。
2. 将以下内容添加为该部分的最后一条项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存更新后的指令后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认已检出 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   我们刚才更新了指令和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何读取指令文件中的上下文，并将新的标准应用于功能中。具体来说，你：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 查看了指令文件对代码的影响。

接下来，[自定义并运行可重复使用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
